# Spring Boot REST API Pattern

## Descripción

Skill para crear endpoints REST API en Spring Boot siguiendo las mejores prácticas del proyecto Myclimbook.

## Cuándo usar

- Crear nuevos controladores REST
- Añadir endpoints a controladores existentes
- Implementar operaciones CRUD
- Gestionar respuestas HTTP

## Patrón estándar

### 1. Controlador REST

```java
@RestController
@RequestMapping("/api/v1/resource")
@RequiredArgsConstructor
public class ResourceController {
    
    private final ResourceService resourceService;
    
    @GetMapping
    public ResponseEntity<List<ResourceDTO>> getAll(
            @RequestParam(defaultValue = "0") int page,
            @RequestParam(defaultValue = "20") int size) {
        List<ResourceDTO> resources = resourceService.findAll(page, size);
        return ResponseEntity.ok(resources);
    }
    
    @GetMapping("/{id}")
    public ResponseEntity<ResourceDTO> getById(@PathVariable Long id) {
        return resourceService.findById(id)
                .map(ResponseEntity::ok)
                .orElse(ResponseEntity.notFound().build());
    }
    
    @PostMapping
    @PreAuthorize("isAuthenticated()")
    public ResponseEntity<ResourceDTO> create(
            @Valid @RequestBody CreateResourceRequest request,
            Authentication authentication) {
        String userId = authentication.getName();
        ResourceDTO created = resourceService.create(request, userId);
        return ResponseEntity.status(HttpStatus.CREATED).body(created);
    }
    
    @PutMapping("/{id}")
    @PreAuthorize("isAuthenticated()")
    public ResponseEntity<ResourceDTO> update(
            @PathVariable Long id,
            @Valid @RequestBody UpdateResourceRequest request,
            Authentication authentication) {
        String userId = authentication.getName();
        return resourceService.update(id, request, userId)
                .map(ResponseEntity::ok)
                .orElse(ResponseEntity.notFound().build());
    }
    
    @DeleteMapping("/{id}")
    @PreAuthorize("isAuthenticated()")
    public ResponseEntity<Void> delete(
            @PathVariable Long id,
            Authentication authentication) {
        String userId = authentication.getName();
        if (resourceService.delete(id, userId)) {
            return ResponseEntity.noContent().build();
        }
        return ResponseEntity.notFound().build();
    }
}
```

### 2. Servicio

```java
@Service
@RequiredArgsConstructor
@Transactional
public class ResourceService {
    
    private final ResourceRepository resourceRepository;
    private final UserRepository userRepository;
    
    @Transactional(readOnly = true)
    public List<ResourceDTO> findAll(int page, int size) {
        Pageable pageable = PageRequest.of(page, size, Sort.by("createdAt").descending());
        return resourceRepository.findAll(pageable)
                .getContent()
                .stream()
                .map(this::toDTO)
                .collect(Collectors.toList());
    }
    
    @Transactional(readOnly = true)
    public Optional<ResourceDTO> findById(Long id) {
        return resourceRepository.findById(id)
                .map(this::toDTO);
    }
    
    public ResourceDTO create(CreateResourceRequest request, String userId) {
        User owner = userRepository.findByUuid(userId)
                .orElseThrow(() -> new ResourceNotFoundException("User not found"));
        
        Resource resource = new Resource();
        resource.setName(request.getName());
        resource.setOwner(owner);
        // ... más campos
        
        Resource saved = resourceRepository.save(resource);
        return toDTO(saved);
    }
    
    public Optional<ResourceDTO> update(Long id, UpdateResourceRequest request, String userId) {
        return resourceRepository.findById(id)
                .filter(resource -> resource.getOwner().getUuid().equals(userId))
                .map(resource -> {
                    resource.setName(request.getName());
                    // ... actualizar campos
                    return toDTO(resourceRepository.save(resource));
                });
    }
    
    public boolean delete(Long id, String userId) {
        return resourceRepository.findById(id)
                .filter(resource -> resource.getOwner().getUuid().equals(userId))
                .map(resource -> {
                    resourceRepository.delete(resource);
                    return true;
                })
                .orElse(false);
    }
    
    private ResourceDTO toDTO(Resource resource) {
        return new ResourceDTO(
                resource.getId(),
                resource.getName(),
                resource.getOwner().getUsername(),
                resource.getCreatedAt()
        );
    }
}
```

### 3. DTOs

```java
// Para respuestas
public record ResourceDTO(
        Long id,
        String name,
        String ownerUsername,
        LocalDateTime createdAt
) {}

// Para peticiones de creación
public record CreateResourceRequest(
        @NotBlank @Size(max = 100) String name,
        @Size(max = 500) String description
) {}

// Para peticiones de actualización
public record UpdateResourceRequest(
        @NotBlank @Size(max = 100) String name,
        @Size(max = 500) String description
) {}
```

## Convenciones

- **URLs**: `/api/v1/resource` (plural, minúsculas)
- **Métodos HTTP**:
  - GET → Leer
  - POST → Crear
  - PUT → Actualizar completo
  - PATCH → Actualizar parcial
  - DELETE → Eliminar
- **Códigos HTTP**:
  - 200 OK → Éxito
  - 201 Created → Recurso creado
  - 204 No Content → Éxito sin respuesta
  - 400 Bad Request → Datos inválidos
  - 401 Unauthorized → No autenticado
  - 403 Forbidden → Sin permisos
  - 404 Not Found → Recurso no existe
  - 500 Internal Server Error → Error del servidor

## Validación

```java
// En el DTO de petición
public record CreateResourceRequest(
        @NotBlank(message = "El nombre es obligatorio")
        @Size(max = 100, message = "Máximo 100 caracteres")
        String name,
        
        @Email(message = "Email inválido")
        String email
) {}

// En el controlador, usar @Valid
@PostMapping
public ResponseEntity<ResourceDTO> create(@Valid @RequestBody CreateResourceRequest request) {
    // Si la validación falla, Spring devuelve 400 automáticamente
}
```

## Manejo de errores

```java
@RestControllerAdvice
public class GlobalExceptionHandler {
    
    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleNotFound(ResourceNotFoundException ex) {
        return ResponseEntity.status(HttpStatus.NOT_FOUND)
                .body(new ErrorResponse("NOT_FOUND", ex.getMessage()));
    }
    
    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<ErrorResponse> handleValidation(MethodArgumentNotValidException ex) {
        List<String> errors = ex.getBindingResult()
                .getFieldErrors()
                .stream()
                .map(error -> error.getField() + ": " + error.getDefaultMessage())
                .collect(Collectors.toList());
        
        return ResponseEntity.status(HttpStatus.BAD_REQUEST)
                .body(new ErrorResponse("VALIDATION_ERROR", errors));
    }
}

public record ErrorResponse(String code, Object message) {}
```

## Ejemplos del proyecto Myclimbook

- `WallController` → CRUD de muros
- `RouteController` → CRUD de rutas
- `ClimbingSectorController` → CRUD de sectores

## Referencias

- Spring Data JPA: https://spring.io/projects/spring-data-jpa
- Spring Security: https://spring.io/projects/spring-security
- Validation: https://docs.spring.io/spring-boot/docs/current/reference/html/io.html#io.validation
