# 💻 Ejemplos de Integración por Tecnología

## 1️⃣ JavaScript Vanilla + Fetch

```javascript
// register.js
document.getElementById('registerForm').addEventListener('submit', async (e) => {
  e.preventDefault();

  const formData = new FormData(e.target);
  const userData = Object.fromEntries(formData);

  try {
    const response = await fetch('http://localhost:8000/users/', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(userData)
    });

    if (!response.ok) {
      const errors = await response.json();
      showErrors(errors);
      return;
    }

    const data = await response.json();
    alert('¡Registro exitoso!');
    console.log('Usuario:', data);
  } catch (error) {
    alert('Error de conexión');
    console.error(error);
  }
});

function showErrors(errors) {
  Object.entries(errors).forEach(([field, messages]) => {
    const element = document.querySelector(`[name="${field}"]`);
    if (element) {
      element.style.borderColor = 'red';
      const error = document.createElement('small');
      error.style.color = 'red';
      error.textContent = messages.join(', ');
      element.after(error);
    }
  });
}
```

---

## 2️⃣ React (Hooks)

```jsx
// RegisterForm.jsx
import { useState } from 'react';

export default function RegisterForm() {
  const [loading, setLoading] = useState(false);
  const [errors, setErrors] = useState({});
  
  const [form, setForm] = useState({
    username: '',
    email: '',
    password: '',
    full_name: '',
    rut: '',
    phone: '',
    commune: '',
    address: ''
  });

  const handleSubmit = async (e) => {
    e.preventDefault();
    setLoading(true);
    setErrors({});

    try {
      const res = await fetch('http://localhost:8000/users/', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(form)
      });

      const data = await res.json();

      if (!res.ok) {
        setErrors(data);
        return;
      }

      // Éxito
      alert('¡Registrado exitosamente!');
      setForm({
        username: '', email: '', password: '',
        full_name: '', rut: '', phone: '', commune: '', address: ''
      });
    } catch (err) {
      console.error('Error:', err);
    } finally {
      setLoading(false);
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        name="full_name"
        placeholder="Nombre Completo"
        value={form.full_name}
        onChange={(e) => setForm({...form, full_name: e.target.value})}
        required
      />
      {errors.full_name && <small style={{color: 'red'}}>{errors.full_name}</small>}

      <input
        type="text"
        name="rut"
        placeholder="RUT (12345678-9)"
        value={form.rut}
        onChange={(e) => setForm({...form, rut: e.target.value})}
        required
      />
      {errors.rut && <small style={{color: 'red'}}>{errors.rut}</small>}

      <input
        type="tel"
        name="phone"
        placeholder="Teléfono (9 1234 5678)"
        value={form.phone}
        onChange={(e) => setForm({...form, phone: e.target.value})}
        required
      />
      {errors.phone && <small style={{color: 'red'}}>{errors.phone}</small>}

      <input
        type="text"
        name="commune"
        placeholder="Comuna"
        value={form.commune}
        onChange={(e) => setForm({...form, commune: e.target.value})}
        required
      />

      <textarea
        name="address"
        placeholder="Dirección"
        value={form.address}
        onChange={(e) => setForm({...form, address: e.target.value})}
        required
      />

      <input
        type="email"
        name="email"
        placeholder="Correo"
        value={form.email}
        onChange={(e) => setForm({...form, email: e.target.value})}
        required
      />

      <input
        type="text"
        name="username"
        placeholder="Nombre de Usuario"
        value={form.username}
        onChange={(e) => setForm({...form, username: e.target.value})}
        required
      />

      <input
        type="password"
        name="password"
        placeholder="Contraseña"
        value={form.password}
        onChange={(e) => setForm({...form, password: e.target.value})}
        required
      />

      <button type="submit" disabled={loading}>
        {loading ? 'Registrando...' : 'Registrar'}
      </button>
    </form>
  );
}
```

---

## 3️⃣ Vue 3 (Composition API)

```vue
<!-- RegisterForm.vue -->
<template>
  <form @submit.prevent="handleSubmit">
    <div>
      <input
        v-model="form.full_name"
        type="text"
        placeholder="Nombre Completo"
        required
      />
      <small v-if="errors.full_name" class="error">{{ errors.full_name }}</small>
    </div>

    <div>
      <input
        v-model="form.rut"
        type="text"
        placeholder="RUT (12345678-9)"
        required
      />
      <small v-if="errors.rut" class="error">{{ errors.rut }}</small>
    </div>

    <div>
      <input
        v-model="form.phone"
        type="tel"
        placeholder="Teléfono (9 1234 5678)"
        required
      />
      <small v-if="errors.phone" class="error">{{ errors.phone }}</small>
    </div>

    <div>
      <input
        v-model="form.commune"
        type="text"
        placeholder="Comuna"
        required
      />
    </div>

    <div>
      <textarea
        v-model="form.address"
        placeholder="Dirección"
        required
      ></textarea>
    </div>

    <div>
      <input
        v-model="form.email"
        type="email"
        placeholder="Correo"
        required
      />
      <small v-if="errors.email" class="error">{{ errors.email }}</small>
    </div>

    <div>
      <input
        v-model="form.username"
        type="text"
        placeholder="Nombre de Usuario"
        required
      />
      <small v-if="errors.username" class="error">{{ errors.username }}</small>
    </div>

    <div>
      <input
        v-model="form.password"
        type="password"
        placeholder="Contraseña"
        required
      />
      <small v-if="errors.password" class="error">{{ errors.password }}</small>
    </div>

    <button :disabled="loading">
      {{ loading ? 'Registrando...' : 'Registrar' }}
    </button>
  </form>
</template>

<script setup>
import { ref } from 'vue';

const form = ref({
  username: '',
  email: '',
  password: '',
  full_name: '',
  rut: '',
  phone: '',
  commune: '',
  address: ''
});

const errors = ref({});
const loading = ref(false);

const handleSubmit = async () => {
  loading.value = true;
  errors.value = {};

  try {
    const response = await fetch('http://localhost:8000/users/', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(form.value)
    });

    const data = await response.json();

    if (!response.ok) {
      errors.value = data;
      return;
    }

    alert('¡Registrado exitosamente!');
    form.value = {
      username: '', email: '', password: '',
      full_name: '', rut: '', phone: '', commune: '', address: ''
    };
  } catch (error) {
    console.error('Error:', error);
  } finally {
    loading.value = false;
  }
};
</script>

<style scoped>
.error {
  color: red;
  display: block;
  margin-top: 5px;
}
</style>
```

---

## 4️⃣ Angular

```typescript
// register.component.ts
import { Component } from '@angular/core';
import { HttpClient } from '@angular/common/http';

@Component({
  selector: 'app-register',
  templateUrl: './register.component.html',
  styleUrls: ['./register.component.css']
})
export class RegisterComponent {
  form = {
    username: '',
    email: '',
    password: '',
    full_name: '',
    rut: '',
    phone: '',
    commune: '',
    address: ''
  };

  errors: any = {};
  loading = false;

  constructor(private http: HttpClient) {}

  onSubmit() {
    this.loading = true;
    this.errors = {};

    this.http.post('http://localhost:8000/users/', this.form)
      .subscribe({
        next: (response) => {
          alert('¡Registrado exitosamente!');
          this.form = {
            username: '', email: '', password: '',
            full_name: '', rut: '', phone: '', commune: '', address: ''
          };
          this.loading = false;
        },
        error: (error) => {
          this.errors = error.error;
          this.loading = false;
        }
      });
  }
}
```

---

## 5️⃣ TypeScript + Axios

```typescript
// auth.service.ts
import axios from 'axios';

interface UserData {
  username: string;
  email: string;
  password: string;
  full_name: string;
  rut: string;
  phone: string;
  commune: string;
  address: string;
}

export async function registerUser(userData: UserData) {
  try {
    const response = await axios.post(
      'http://localhost:8000/users/',
      userData,
      {
        headers: {
          'Content-Type': 'application/json'
        }
      }
    );

    return {
      success: true,
      data: response.data
    };
  } catch (error: any) {
    return {
      success: false,
      errors: error.response?.data || { message: 'Error de conexión' }
    };
  }
}
```

---

## 6️⃣ cURL (Testing)

```bash
curl -X POST http://localhost:8000/users/ \
  -H "Content-Type: application/json" \
  -d '{
    "username": "testuser",
    "email": "test@example.com",
    "password": "TestPass123!",
    "full_name": "Test User",
    "rut": "12345678-9",
    "phone": "9 1234 5678",
    "commune": "Santiago",
    "address": "Calle Test 123"
  }'
```

---

## 7️⃣ Python + Requests

```python
# register.py
import requests
import json

def register_user(user_data):
    url = 'http://localhost:8000/users/'
    
    try:
        response = requests.post(
            url,
            json=user_data,
            headers={'Content-Type': 'application/json'}
        )
        
        if response.status_code == 201:
            print("✅ Registro exitoso")
            print(json.dumps(response.json(), indent=2))
        else:
            print("❌ Error:", response.json())
            
    except requests.exceptions.RequestException as e:
        print(f"❌ Error de conexión: {e}")


# Uso
user_data = {
    'username': 'testuser',
    'email': 'test@example.com',
    'password': 'TestPass123!',
    'full_name': 'Test User',
    'rut': '12345678-9',
    'phone': '9 1234 5678',
    'commune': 'Santiago',
    'address': 'Calle Test 123'
}

register_user(user_data)
```

---

## 🚀 Quick Start para tu Frontend

1. Elige la tecnología que uses
2. Copia el código del ejemplo
3. Reemplaza `http://localhost:8000` si tu backend está en otro servidor
4. Maneja los errores específicamente
5. ¡Listo!

---

## 📝 Notas Importantes

- Todos los campos son **REQUERIDOS**
- Los formatos de RUT y teléfono son **VALIDADOS**
- Las contraseñas se **HASHEAN** automáticamente
- Los campos username, email y rut deben ser **ÚNICOS**
