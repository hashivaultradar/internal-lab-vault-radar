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

###  Prerrequisitos

1. **Vault Radar CLI instalado**
   ```bash
   # Verificar si está instalado
   vault-radar --version
   
   # Si no está instalado, seguir las instrucciones oficiales:
   # https://developer.hashicorp.com/hcp/docs/vault-radar/cli#Offline-mode
   ```

2. **Acceso a HashiCorp Cloud Platform (HCP)**
   - Organización de HCP configurada
   - Proyecto de HCP creado
   - Credenciales de HCP (Client ID y Secret)

### Paso 1: Configurar Variables de Entorno

Configura las siguientes variables de entorno de HCP (HashiCorp Cloud Platform) en tu terminal:

```bash
export HCP_CLIENT_ID="TU_CLIENT_ID_AQUI"
export HCP_CLIENT_SECRET="TU_CLIENT_SECRET_AQUI"
export HCP_ORGANIZATION_ID="TU_ORGANIZATION_ID_AQUI"
export HCP_PROJECT_ID="TU_PROJECT_ID_AQUI"
```


### Paso 2: Crear el Pre-commit Hook

Crea el archivo del hook en `.git/hooks/pre-commit`:

```bash
# Asegúrate de estar en la raíz del repositorio
cd /ruta/a/tu/repositorio

# Crear el directorio de hooks si no existe
mkdir -p .git/hooks

# Crear el archivo pre-commit
cat > .git/hooks/pre-commit << 'EOF'
#!/bin/sh
echo ""
echo "🔍 Escaneando con Vault Radar..."
echo ""

# Ejecutar el scan de Vault Radar
OUTPUT=$(vault-radar scan git pre-commit 2>&1)
EXIT_CODE=$?

# Mostrar la salida
echo "$OUTPUT"

# Verificar si se detectaron secretos
if echo "$OUTPUT" | grep -qi "detected"; then
    echo ""
    echo "⚠️  WARNING: Vault Radar encontró secretos en tu commit."
    echo "⚠️  Este commit se permitirá para propósitos de demostración."
    echo "⚠️  En producción, estos commits deberían ser bloqueados."
    echo ""
    echo ""
fi

echo "✅ Commit permitido (modo demostración)"
echo ""
echo ""
exit 0
EOF

# Dar permisos de ejecución al hook
chmod +x .git/hooks/pre-commit
```

### Paso 3: Verificar que el Hook Funciona

Prueba el hook haciendo un commit de prueba:

```bash
# Hacer un cambio pequeño
echo "# Test" >> test-file.md

# Intentar hacer commit (esto debería ejecutar el hook)
git add test-file.md
git commit -m "Test: verificar pre-commit hook"

# Deberías ver la salida del escaneo de Vault Radar
```
