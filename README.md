# Subasta de oficina

Subasta ascendente con turnos aleatorios, para jugar entre compañeros desde el celular.
Un solo archivo `index.html`, sin build. El estado compartido vive en Firestore.

## 1. Crear el proyecto de Firebase

1. Entra a la consola de Firebase y crea un proyecto (o reutiliza uno existente).
2. **Authentication → Sign-in method → Anonymous → Habilitar.**
   Sin esto la app no arranca: las reglas exigen sesión.
3. **Firestore Database → Crear base de datos → modo producción.**
   Elige la región `nam5` o `southamerica-east1`.
4. **Configuración del proyecto → Tus apps → Web (`</>`)** y copia el objeto
   `firebaseConfig`.

## 2. Pegar la configuración

Abre `index.html`, busca el bloque `CONFIGURACIÓN` (arriba del todo, dentro del
`<script>`) y reemplaza los valores de `firebaseConfig` por los tuyos.

Las claves de Firebase Web son públicas por diseño: no son un secreto, quien
proteja los datos son las reglas de Firestore.

## 3. Publicar las reglas

Copia el contenido de `firestore.rules` en
**Firestore Database → Reglas → Publicar.**

## 4. Subir a GitHub Pages

```bash
git init
git add index.html firestore.rules README.md
git commit -m "Subasta de oficina"
git branch -M main
git remote add origin https://github.com/TU_USUARIO/TU_REPO.git
git push -u origin main
```

En el repo: **Settings → Pages → Source: Deploy from a branch → `main` / `root`.**

En 1–2 minutos queda en `https://TU_USUARIO.github.io/TU_REPO/`.

## 5. Usarlo

1. Abre la URL, entra como **organizador**, llena el formulario y define un PIN.
2. Usa **Copiar enlace para el grupo** y pégalo en el WhatsApp.
3. Cada persona abre el enlace, entra como **participante** y elige su nombre.
4. Cuando estén todos, el organizador toca **Iniciar subasta**.

### Varias subastas a la vez

Agrega `?sala=loquesea` a la URL:
`https://TU_USUARIO.github.io/TU_REPO/?sala=camioneta`.
Cada sala es un documento independiente en Firestore.

## Notas importantes

- **El PIN no es seguridad real.** Protege contra el compañero curioso que toca
  "Soy el organizador" por accidente, no contra alguien que sepa abrir las
  herramientas de desarrollador. Si necesitas control de verdad, cambia la auth
  anónima por Google Sign-In y valida tu correo en las reglas.
- **Cualquiera puede elegir el nombre de otro** en la pantalla de identificación.
  Con 35 compañeros en una oficina normalmente basta la confianza; si no, hace
  falta autenticación real.
- **El reloj del turno usa la hora del celular.** Si alguien tiene la hora
  desconfigurada manualmente verá la cuenta regresiva corrida. Con la hora
  automática (lo normal) la diferencia es de menos de un segundo.
- **Costo:** una subasta de 35 personas gasta del orden de unos pocos miles de
  lecturas, muy por debajo del nivel gratuito de Firestore (50.000 lecturas y
  20.000 escrituras por día).
- **Requiere internet.** La app carga React, Tailwind y Babel desde CDN.
