# JPA Entity Pattern

## Descripción

Skill para crear entidades JPA en Spring Boot con las convenciones del proyecto Myclimbook.

## Cuándo usar

- Crear nuevas entidades de base de datos
- Definir relaciones entre entidades
- Configurar mappings JPA

## Patrón estándar

### 1. Entidad simple

```java
@Entity
@Table(name = "walls")
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class Wall {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(name = "uuid", nullable = false, unique = true, length = 36)
    private String uuid;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "owner_id", nullable = false)
    private User owner;

    @Column(nullable = false, length = 100)
    private String name;

    @Column(name = "image_url", length = 500)
    private String imageUrl;

    @Enumerated(EnumType.STRING)
    @Column(nullable = false, length = 20)
    private WallArtwork artwork;

    @Column(name = "is_public", nullable = false)
    private Boolean isPublic = true;

    @Column(name = "route_count", nullable = false)
    private Integer routeCount = 0;

    @Column(name = "created_at", nullable = false, updatable = false)
    private LocalDateTime createdAt;

    @PrePersist
    protected void onCreate() {
        if (uuid == null) {
            uuid = UUID.randomUUID().toString();
        }
        createdAt = LocalDateTime.now();
    }
}
```

### 2. Entidad con relaciones

```java
@Entity
@Table(name = "climbing_routes")
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class ClimbingRoute {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(name = "uuid", nullable = false, unique = true, length = 36)
    private String uuid;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "wall_id", nullable = false)
    private Wall wall;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "owner_id", nullable = false)
    private User owner;

    @Column(nullable = false, length = 100)
    private String name;

    @Column(name = "grade_v", length = 10)
    private String gradeV;

    @Column(name = "grade_font", length = 10)
    private String gradeFont;

    @Column(name = "average_rating", precision = 2, scale = 1)
    private Double averageRating = 0.0;

    @Column(name = "ratings_count")
    private Integer ratingsCount = 0;

    @Column(name = "comment_count")
    private Integer commentCount = 0;

    @OneToMany(mappedBy = "route", cascade = CascadeType.ALL, orphanRemoval = true)
    @Builder.Default
    private List<RouteMarker> markers = new ArrayList<>();

    @OneToMany(mappedBy = "contentId", cascade = CascadeType.ALL)
    @Builder.Default
    private List<Comment> comments = new ArrayList<>();

    @Column(name = "created_at", nullable = false, updatable = false)
    private LocalDateTime createdAt;

    @PrePersist
    protected void onCreate() {
        if (uuid == null) {
            uuid = UUID.randomUUID().toString();
        }
        createdAt = LocalDateTime.now();
    }

    // Métodos helper para mantener relaciones
    public void addMarker(RouteMarker marker) {
        markers.add(marker);
        marker.setRoute(this);
    }

    public void removeMarker(RouteMarker marker) {
        markers.remove(marker);
        marker.setRoute(null);
    }
}
```

### 3. Enumeraciones

```java
public enum WallArtwork {
    SUNSET,
    METAL,
    CANYON,
    NIGHT
}

public enum MarkerType {
    START,
    FOOT,
    HAND,
    TOP,
    BOLT,
    ANCHOR
}

public enum AnchorType {
    NONE,
    CHAIN,
    RINGS,
    CARABINER,
    OTHER
}
```

### 4. Repository

```java
@Repository
public interface WallRepository extends JpaRepository<Wall, Long> {

    // Búsqueda por UUID
    Optional<Wall> findByUuid(String uuid);

    // Búsqueda por propietario
    List<Wall> findByOwner_Id(Long ownerId);

    // Muros públicos ordenados por fecha
    @Query("SELECT w FROM Wall w WHERE w.isPublic = true ORDER BY w.createdAt DESC")
    Page<Wall> findPublicWalls(Pageable pageable);

    // Búsqueda por nombre (case insensitive)
    @Query("SELECT w FROM Wall w WHERE LOWER(w.name) LIKE LOWER(CONCAT('%', :query, '%'))")
    List<Wall> searchByName(@Param("query") String query);

    // Contar rutas de un muro
    @Query("SELECT COUNT(r) FROM ClimbingRoute r WHERE r.wall.id = :wallId")
    int countRoutesByWallId(@Param("wallId") Long wallId);

    // Muros públicos de un usuario
    @Query("SELECT w FROM Wall w WHERE w.owner.id = :userId AND w.isPublic = true")
    List<Wall> findPublicByUserId(@Param("userId") Long userId);
}
```

## Convenciones

### Nombres de tablas
- **Plural**: `walls`, `users`, `climbing_routes`
- **Snake case**: `sport_routes`, `boulder_problems`

### Nombres de columnas
- **Snake case**: `owner_id`, `created_at`, `average_rating`
- **Booleanos**: `is_public`, `is_banned`

### Relaciones

#### One-to-Many (uno a muchos)
```java
@OneToMany(mappedBy = "wall", cascade = CascadeType.ALL, orphanRemoval = true)
private List<ClimbingRoute> routes;
```

#### Many-to-One (muchos a uno)
```java
@ManyToOne(fetch = FetchType.LAZY)
@JoinColumn(name = "wall_id", nullable = false)
private Wall wall;
```

#### Many-to-Many (muchos a muchos)
```java
@ManyToMany
@JoinTable(
    name = "user_favorites",
    joinColumns = @JoinColumn(name = "user_id"),
    inverseJoinColumns = @JoinColumn(name = "wall_id")
)
private Set<Wall> favoriteWalls;
```

### Fetch Types
- **LAZY** (recomendado): Carga solo cuando se accede
- **EAGER**: Carga siempre (evitar en relaciones grandes)

### Cascade Types
- **ALL**: Propaga todas las operaciones
- **PERSIST**: Propaga solo persistencia
- **REMOVE**: Propaga solo eliminación
- **orphanRemoval**: Elimina hijos huérfanos

## Índices

```java
@Entity
@Table(name = "walls", indexes = {
    @Index(name = "idx_wall_owner", columnList = "owner_id"),
    @Index(name = "idx_wall_created", columnList = "created_at"),
    @Index(name = "idx_wall_public", columnList = "is_public")
})
public class Wall {
    // ...
}
```

## Validaciones en entidad

```java
@Column(nullable = false, length = 100)
@NotBlank(message = "El nombre es obligatorio")
@Size(max = 100, message = "Máximo 100 caracteres")
private String name;

@Column(name = "average_rating", precision = 2, scale = 1)
@DecimalMin(value = "0.0", message = "Mínimo 0.0")
@DecimalMax(value = "5.0", message = "Máximo 5.0")
private Double averageRating;
```

## Métodos helper

```java
public class Wall {
    // ... campos

    public void incrementRouteCount() {
        this.routeCount++;
    }

    public void decrementRouteCount() {
        if (this.routeCount > 0) {
            this.routeCount--;
        }
    }

    public boolean isOwnedBy(String userId) {
        return this.owner.getUuid().equals(userId);
    }
}
```

## Ejemplos del proyecto Myclimbook

- `Wall` → Muros de escalada
- `ClimbingRoute` → Rutas en muros
- `ClimbingSector` → Sectores en roca
- `SportRoute` → Vías deportivas
- `BoulderProblem` → Problemas de búlder
- `User` → Usuarios
- `Comment` → Comentarios
- `Rating` → Valoraciones

## Referencias

- JPA Specification: https://jakarta.ee/specifications/persistence/
- Spring Data JPA: https://spring.io/projects/spring-data-jpa
- Hibernate: https://hibernate.org/orm/
