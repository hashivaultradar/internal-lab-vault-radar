#  Internal Lab - Vault Radar

Este repositorio contiene **credenciales expuestas INTENCIONALMENTE** para pruebas de detección de secretos con **HashiCorp Vault Radar**.


##  Estructura del Proyecto

```
├── config/
│   ├── aws.js              # Credenciales AWS
│   ├── db.js               # Credenciales PostgreSQL
│   ├── db-connection-string.js  # Connection string PostgreSQL
│   ├── jwt.js              # Secreto JWT
│   └── deploy.yml          # Pipeline CI/CD con secretos
└── README.md
```

#  Opcional


##  Configurar pre commit

Se usan variables de HCP (HashiCorp Cloud Platform):

export HCP_CLIENT_ID=
export HCP_CLIENT_SECRET=
export HCP_ORGANIZATION_ID=
export HCP_PROJECT_ID=

...