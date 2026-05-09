# 📋 Instrucciones para Conectar Frontend con UserService

## Endpoint de Registro (POST)

**URL:** `http://localhost:8000/users/`

**Método:** `POST`

**Content-Type:** `application/json`

---

## Campos Requeridos

| Campo | Tipo | Descripción | Ejemplo |
|-------|------|-------------|---------|
| `username` | string | Nombre de usuario único | `"juan.perez"` |
| `email` | string | Email único | `"juan@example.com"` |
| `password` | string | Contraseña (mín 8 caracteres) | `"SecurePass123!"` |
| `full_name` | string | Nombre completo | `"Juan Carlos Pérez López"` |
| `rut` | string | RUT chileno con formato | `"12345678-9"` o `"12.345.678-9"` |
| `phone` | string | Teléfono chileno | `"9 1234 5678"` o `"+56 9 1234 5678"` o `"912345678"` |
| `commune` | string | Comuna/Municipio | `"Santiago"` |
| `address` | string | Dirección completa | `"Calle Principal 123, Piso 4"` |

---

## Formato de Teléfono Aceptado

✅ **Formatos válidos:**
- `9 1234 5678` (con espacios)
- `+56 9 1234 5678` (formato internacional)
- `912345678` (sin espacios)

---

## Formato de RUT Aceptado

✅ **Formatos válidos:**
- `12345678-9` (solo guion)
- `12.345.678-9` (con puntos y guion)

⚠️ **El dígito verificador será validado automáticamente**

---

## Ejemplo de Petición

```javascript
fetch('http://localhost:8000/users/', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    username: 'juan.perez',
    email: 'juan@example.com',
    password: 'SecurePass123!',
    full_name: 'Juan Carlos Pérez López',
    rut: '12345678-9',
    phone: '9 1234 5678',
    commune: 'Santiago',
    address: 'Calle Principal 123, Piso 4'
  })
})
.then(response => response.json())
.then(data => console.log('Usuario registrado:', data))
.catch(error => console.error('Error:', error));
```

---

## Respuesta Exitosa (201 Created)

```json
{
  "id": 1,
  "username": "juan.perez",
  "email": "juan@example.com",
  "full_name": "Juan Carlos Pérez López",
  "rut": "12345678-9",
  "phone": "9 1234 5678",
  "commune": "Santiago",
  "address": "Calle Principal 123, Piso 4"
}
```

---

## Errores Comunes

### ❌ RUT Inválido
```json
{
  "rut": [
    "RUT inválido: dígito verificador incorrecto"
  ]
}
```
**Solución:** Verifica que el dígito verificador sea correcto

---

### ❌ Teléfono Inválido
```json
{
  "phone": [
    "Teléfono debe estar en formato: 9 XXXX XXXX, +56 9 XXXX XXXX, o 9XXXXXXXX"
  ]
}
```
**Solución:** Usa uno de los tres formatos válidos

---

### ❌ Usuario o Email Duplicado
```json
{
  "username": [
    "A user with that username already exists."
  ],
  "email": [
    "user with this email address already exists."
  ],
  "rut": [
    "user with this rut already exists."
  ]
}
```
**Solución:** Usa valores únicos

---

### ❌ Contraseña muy corta
```json
{
  "password": [
    "Ensure this field has at least 8 characters."
  ]
}
```
**Solución:** Usa contraseñas de al menos 8 caracteres

---

## Validaciones del Backend

El sistema validará automáticamente:

✅ **RUT chileno:** Formato y dígito verificador  
✅ **Teléfono:** Formato chileno (9 dígitos)  
✅ **Email:** Formato válido de correo  
✅ **Contraseña:** Mínimo 8 caracteres, hasheada con bcrypt  
✅ **Unicidad:** username, email y rut no pueden repetirse

---

## CORS (Si estás en otro puerto)

Si el frontend está en `http://localhost:3000`, agrega a `settings.py`:

```python
# settings.py
INSTALLED_APPS = [
    # ...
    'corsheaders',
]

MIDDLEWARE = [
    'corsheaders.middleware.CorsMiddleware',
    'django.middleware.common.CommonMiddleware',
    # ...
]

CORS_ALLOWED_ORIGINS = [
    'http://localhost:3000',
    'http://localhost:5173',  # Vite
]
```

Instala: `pip install django-cors-headers`

---

## Testing con cURL

```bash
curl -X POST http://localhost:8000/users/ \
  -H "Content-Type: application/json" \
  -d '{
    "username": "juan.perez",
    "email": "juan@example.com",
    "password": "SecurePass123!",
    "full_name": "Juan Carlos Pérez López",
    "rut": "12345678-9",
    "phone": "9 1234 5678",
    "commune": "Santiago",
    "address": "Calle Principal 123, Piso 4"
  }'
```

---

## Estado del Servidor

Para verificar que el servidor esté corriendo:

```bash
# En la carpeta UserService
python manage.py runserver
```

Debería mostrar:
```
Starting development server at http://127.0.0.1:8000/
```
