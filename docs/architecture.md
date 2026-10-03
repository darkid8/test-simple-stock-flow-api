# Arquitectura de la API (Backend Laravel)

El repositorio de la API debe acatar la estructura Onion en 4 capas estrictas, sirviendo como núcleo de reglas del sistema "Simple Stock Flow".

## Estructura de Directorios Aprobada
```text
app/
├── Domain/                             (CAPA 1: NÚCLEO - PHP Puro)
│   ├── Entities/                       (Ej. Product, Sale)
│   ├── ValueObjects/
│   ├── Repositories/                   (Interfaces / Contratos)
│   └── Exceptions/
├── DomainServices/                     (CAPA 2: SERVICIOS DE DOMINIO)
├── Application/                        (CAPA 3: CASOS DE USO)
│   ├── UseCases/
│   └── DTOs/
└── Infrastructure/                     (CAPA 4: INFRAESTRUCTURA & PRESENTACIÓN)
    ├── Persistence/Eloquent/           (Modelos y Mappers)
    ├── Security/
    └── Storage/
```

## Las Reglas Intransigibles
1. **Dominio Virgen**: Ningún archivo bajo `app/Domain` debe importar `Illuminate\*`. Todo código aquí debe poder verificarse con `php -l` sin requerir autoload de Laravel.
2. **Aislamiento de Transacciones**: Los Casos de Uso utilizarán un `TransactionManagerInterface`. Queda prohibido el uso de `DB::transaction()` en `app/Application`.
3. **Propiedad Única de Migraciones**: La base de datos es gestionada exclusivamente por este repositorio (API) a través de `database/migrations/`. 
4. **Protección de Atributos Derivados**: Totales y subtotales no se almacenan como columnas redundantes. Se calculan dinámicamente según ADR-002 y Artículo VII.
