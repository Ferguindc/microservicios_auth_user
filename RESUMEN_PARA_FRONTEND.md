# 🚀 Resumen para Frontend - API Registro de Usuarios

## ✅ Lo que está listo

Tu backend ya tiene un endpoint de registro completamente funcional con validaciones chilenas integradas.

---

## 📍 Endpoint Principal

```
POST http://localhost:8000/users/
```

---

## 📦 Campos del Registro

```javascript
{
  "username": "string (único)",           // ej: "juan.perez"
  "email": "string (único, válido)",      // ej: "juan@example.com"
  "password": "string (min 8 chars)",     // ej: "SecurePass123!"
  "full_name": "string",                  // ej: "Juan Carlos Pérez López"
  "rut": "string (validado)",             // ej: "12345678-9"
  "phone": "string (validado)",           // ej: "9 1234 5678"
  "commune": "string",                    // ej: "Santiago"
  "address": "string"                     // ej: "Calle Principal 123"
}
```

---

## 🛠️ Formatos Específicos Chilenos

### RUT
✅ **Formatos aceptados:**
- `12345678-9` 
- `12.345.678-9`

⚠️ El dígito verificador se valida automáticamente

### Teléfono
✅ **Formatos aceptados:**
- `9 1234 5678`
- `+56 9 1234 5678`
- `912345678`

---

## 📡 Ejemplo Completo con Fetch

```javascript
async function registrarUsuario() {
  const userData = {
    username: 'juan.perez',
    email: 'juan@example.com',
    password: 'SecurePass123!',
    full_name: 'Juan Carlos Pérez López',
    rut: '12345678-9',
    phone: '9 1234 5678',
    commune: 'Santiago',
    address: 'Calle Principal 123, Piso 4'
  };

  try {
    const response = await fetch('http://localhost:8000/users/', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(userData)
    });

    const data = await response.json();

    if (response.ok) {
      console.log('✅ Usuario registrado:', data);
      // Guardar el usuario o redirigir
    } else {
      console.error('❌ Error:', data);
      // Mostrar errores específicos al usuario
    }
  } catch (error) {
    console.error('❌ Error de conexión:', error);
  }
}
```

---

## 🎨 Ejemplo React (Completo)

Usa el archivo `EJEMPLO_REACT.jsx` en la raíz del proyecto. Copiar y pegar directamente en tu aplicación.

---

## ✨ Respuesta Exitosa

**Status Code:** `201 Created`

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

## ⚠️ Errores Posibles

### Email duplicado
```json
{
  "email": ["user with this email address already exists."]
}
```

### RUT inválido
```json
{
  "rut": ["RUT inválido: dígito verificador incorrecto"]
}
```

### Teléfono mal formateado
```json
{
  "phone": ["Teléfono debe estar en formato: 9 XXXX XXXX, +56 9 XXXX XXXX, o 9XXXXXXXX"]
}
```

### Contraseña muy corta
```json
{
  "password": ["Ensure this field has at least 8 characters."]
}
```

---

## 🔧 Cómo Iniciar el Backend

```bash
cd UserService
python manage.py runserver
```

Se ejecutará en: `http://localhost:8000`

---

## 📋 Checklist Implementado

- ✅ Campo: Nombre Completo
- ✅ Campo: RUT con validación chilena
- ✅ Campo: Teléfono con validación chilena
- ✅ Campo: Comuna
- ✅ Campo: Dirección
- ✅ Campo: Correo Electrónico
- ✅ Campo: Contraseña (hasheada con bcrypt)
- ✅ CORS habilitado
- ✅ Validaciones de formato
- ✅ Validaciones de unicidad (username, email, rut)

---

## 🌐 URLs Permitidas en CORS

- `http://localhost:3000` (React por defecto)
- `http://localhost:5173` (Vite por defecto)

Si tu frontend está en otro puerto, notificame para agregarlo.

---

## 📚 Documentación Completa

Ver: `INSTRUCCIONES_FRONTEND.md`
