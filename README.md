# tuplas-typescript

# 📌 **Aprendiendo Tuplas en TypeScript con `as const` y Tipos Derivados**

En este ejemplo, veremos cómo definir y usar **tuplas en TypeScript** con `as const`, además de cómo derivar tipos a partir de un array de valores constantes.

---

## 📌 **Definición de una tupla con `as const`**  
```typescript
const timeZones = [
    'America/Santiago',
    'America/Bogota',
    'Europe/Berlin'
] as const;
```
### 🔍 **¿Qué hace `as const` aquí?**  
- Convierte el array en un **tuple literal**, es decir, **sus valores son inmutables y TypeScript los trata como valores exactos**.
- Sin `as const`, TypeScript trataría `timeZones` como un `string[]`, permitiendo cualquier string en lugar de los valores específicos.

🔹 **Sin `as const`**  
```typescript
const timeZones = ['America/Santiago', 'America/Bogota', 'Europe/Berlin'];
// TypeScript lo interpreta como: string[]
```

🔹 **Con `as const`**  
```typescript
const timeZones = ['America/Santiago', 'America/Bogota', 'Europe/Berlin'] as const;
// TypeScript lo interpreta como: readonly ["America/Santiago", "America/Bogota", "Europe/Berlin"]
```
➡️ Esto impide modificar los valores y asegura que TypeScript los reconozca como valores **literalmente exactos**.

---

## 📌 **Derivando un tipo a partir de la tupla**
```typescript
type TimeZone = (typeof timeZones)[number];
```
### 🔍 **¿Qué significa esto?**  
- `typeof timeZones` obtiene el **tipo del array**, que es:  
  ```typescript
  readonly ["America/Santiago", "America/Bogota", "Europe/Berlin"]
  ```
- `[number]` permite acceder a **cualquier índice del array**, por lo que **TypeScript genera un tipo union** con sus valores.

🔹 **Equivalente a esto:**
```typescript
type TimeZone = "America/Santiago" | "America/Bogota" | "Europe/Berlin";
```
➡️ Ahora, `TimeZone` **solo puede tomar uno de estos valores exactos**, evitando valores no deseados como `"America/Lima"`.

---

## 📌 **Ejemplo de uso en una función**
```typescript
export function getDateInTimeZone(timeZone: TimeZone) {
    return new Date(new Date().toLocaleString('en-US', { timeZone }));
}
```
### 🔍 **¿Cómo funciona?**  
1. La función **recibe una zona horaria**, pero **TypeScript restringe los valores** al tipo `TimeZone`.
2. `new Date().toLocaleString('en-US', { timeZone })` obtiene la fecha y hora en la zona horaria especificada.
3. Se crea una nueva instancia de `Date` basada en la cadena formateada, garantizando que sea un objeto `Date` válido.

### 🛠 **Ejemplo de uso:**
```typescript
console.log(getDateInTimeZone("America/Bogota"));  // ✅ Funciona
console.log(getDateInTimeZone("Europe/Berlin"));  // ✅ Funciona
console.log(getDateInTimeZone("America/Lima"));   // ❌ Error en TypeScript: "America/Lima" no está en TimeZone
```

---

## 🚀 **Beneficios de este enfoque**
✔ **Más seguro**: TypeScript detecta errores si usamos un valor incorrecto.  
✔ **Más preciso**: Evita permitir valores inesperados en la función.  
✔ **Más limpio**: No necesitas definir manualmente `type TimeZone = "..." | "..."`, el tipo se genera automáticamente.  

🔹 **Cuando usar `as const` y `[number]`?**  
Este patrón es útil cuando quieres **definir valores fijos y generar un tipo automáticamente a partir de ellos** sin escribir manualmente todas las opciones.

---

💡 **Conclusión:**  
Usar `as const` junto con `[number]` en TypeScript es una excelente manera de **crear tipos seguros y evitar errores en valores restringidos**. Es especialmente útil en **configuraciones, enums simulados y listas de opciones predefinidas**.

