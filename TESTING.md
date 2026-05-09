# 🧪 Testing del Endpoint de Registro

## 1. Verificar que el servidor esté corriendo

```bash
cd UserService
python manage.py runserver
```

Deberías ver:
```
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

---

## 2. Testear con cURL

### ✅ Registro exitoso

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

**Respuesta esperada (201 Created):**
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

### ❌ Test de validaciones

#### RUT inválido (dígito verificador incorrecto)

```bash
curl -X POST http://localhost:8000/users/ \
  -H "Content-Type: application/json" \
  -d '{
    "username": "test1",
    "email": "test1@example.com",
    "password": "SecurePass123!",
    "full_name": "Test User",
    "rut": "12345678-0",
    "phone": "9 1234 5678",
    "commune": "Santiago",
    "address": "Test address"
  }'
```

**Respuesta:**
```json
{
  "rut": ["RUT inválido: dígito verificador incorrecto"]
}
```

---

#### Teléfono inválido

```bash
curl -X POST http://localhost:8000/users/ \
  -H "Content-Type: application/json" \
  -d '{
    "username": "test2",
    "email": "test2@example.com",
    "password": "SecurePass123!",
    "full_name": "Test User",
    "rut": "12345678-9",
    "phone": "1234567",
    "commune": "Santiago",
    "address": "Test address"
  }'
```

**Respuesta:**
```json
{
  "phone": ["Teléfono debe estar en formato: 9 XXXX XXXX, +56 9 XXXX XXXX, o 9XXXXXXXX"]
}
```

---

#### Email inválido

```bash
curl -X POST http://localhost:8000/users/ \
  -H "Content-Type: application/json" \
  -d '{
    "username": "test3",
    "email": "notanemail",
    "password": "SecurePass123!",
    "full_name": "Test User",
    "rut": "12345678-9",
    "phone": "9 1234 5678",
    "commune": "Santiago",
    "address": "Test address"
  }'
```

**Respuesta:**
```json
{
  "email": ["Enter a valid email address."]
}
```

---

#### Contraseña muy corta

```bash
curl -X POST http://localhost:8000/users/ \
  -H "Content-Type: application/json" \
  -d '{
    "username": "test4",
    "email": "test4@example.com",
    "password": "123",
    "full_name": "Test User",
    "rut": "12345678-9",
    "phone": "9 1234 5678",
    "commune": "Santiago",
    "address": "Test address"
  }'
```

**Respuesta:**
```json
{
  "password": ["Ensure this field has at least 8 characters."]
}
```

---

#### Campos faltantes

```bash
curl -X POST http://localhost:8000/users/ \
  -H "Content-Type: application/json" \
  -d '{
    "username": "test5",
    "email": "test5@example.com",
    "password": "SecurePass123!"
  }'
```

**Respuesta:**
```json
{
  "full_name": ["This field is required."],
  "rut": ["This field is required."],
  "phone": ["This field is required."],
  "commune": ["This field is required."],
  "address": ["This field is required."]
}
```

---

## 3. Testear con Postman

### Crear una nueva request

1. **Método:** POST
2. **URL:** `http://localhost:8000/users/`
3. **Headers:**
   - Key: `Content-Type`
   - Value: `application/json`

4. **Body (raw JSON):**
```json
{
  "username": "juan.perez",
  "email": "juan@example.com",
  "password": "SecurePass123!",
  "full_name": "Juan Carlos Pérez López",
  "rut": "12345678-9",
  "phone": "9 1234 5678",
  "commune": "Santiago",
  "address": "Calle Principal 123, Piso 4"
}
```

5. **Click en Send**

---

## 4. Testear con Python

```python
import requests
import json

# Datos de prueba
user_data = {
    'username': 'juan.perez',
    'email': 'juan@example.com',
    'password': 'SecurePass123!',
    'full_name': 'Juan Carlos Pérez López',
    'rut': '12345678-9',
    'phone': '9 1234 5678',
    'commune': 'Santiago',
    'address': 'Calle Principal 123, Piso 4'
}

# Hacer la petición
response = requests.post(
    'http://localhost:8000/users/',
    json=user_data,
    headers={'Content-Type': 'application/json'}
)

# Mostrar resultado
print(f"Status Code: {response.status_code}")
print(f"Response: {json.dumps(response.json(), indent=2)}")
```

---

## 5. Testear con JavaScript (Browser Console)

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
.then(data => console.log(data))
.catch(error => console.error('Error:', error))
```

---

## 6. RUTs Válidos para Testing

Puedes usar estos RUTs reales que tienen dígitos verificadores correctos:

| RUT | Formato 2 |
|-----|-----------|
| 8-3 | 8-3 |
| 11-6 | 11-6 |
| 11-7 | 11-7 |
| 12-0 | 12-0 |
| 13-6 | 13-6 |
| 14-5 | 14-5 |

**Para testing simple, usa:** `12345678-9`
(Este RUT tiene un dígito verificador válido)

---

## 7. Verificar que el usuario se guardó

### En el Admin de Django

1. Ir a: `http://localhost:8000/admin/`
2. Loguear con credenciales de superusuario
3. Ir a "Users" para ver todos los registros

---

### Con Django shell

```bash
python manage.py shell
```

Luego en la shell:
```python
from users.models import User

# Ver todos los usuarios
User.objects.all()

# Ver un usuario específico
user = User.objects.get(username='juan.perez')
print(user.full_name)
print(user.rut)
print(user.phone)
print(user.commune)
print(user.address)
```

---

## 🚀 Checklist de Testing

- [ ] ✅ Servidor corriendo en puerto 8000
- [ ] ✅ Puedo registrar un usuario válido
- [ ] ✅ Recibo status 201 en éxito
- [ ] ✅ Recibo validación de RUT
- [ ] ✅ Recibo validación de teléfono
- [ ] ✅ Recibo validación de campos requeridos
- [ ] ✅ El usuario se guardó en la BD
- [ ] ✅ CORS funciona desde el frontend

---

## 📞 Troubleshooting

### Error: "Connection refused"
- Asegúrate que el servidor esté corriendo
- Verifica el puerto correcto (8000)

### Error: "CORS error"
- Verifica que CORS esté habilitado en settings.py
- Comprueba que tu puerto frontend esté en CORS_ALLOWED_ORIGINS

### Error: "Migration problems"
- Ejecuta: `python manage.py migrate`

### La base de datos no se actualiza
- Verifica que el servidor esté corriendo
- Revisa la consola de Django para errores
