# Ejercicio: Creación y Uso de Flujos de Trabajo Reutilizables (Reusable Workflows)

## Objetivo
Familiarizarse con la creación y uso de flujos de trabajo reutilizables en GitHub Actions.

---

## Tareas

### 1. Crear un archivo de flujo de trabajo reutilizable
- **Archivo:** `.github/workflows/18-1-reusable-workflow.yaml`
- **Especificaciones del flujo de trabajo reutilizable:**
  - **Nombre:** `18 - 1 - Reusable Workflows - Reusable Definition`
  - **Desencadenantes:**
    - `workflow_call`, que debe:
      - Aceptar un único **input**.
      - Producir dos **outputs**.

#### Detalles del flujo de trabajo:

#### Inputs
| Nombre             | Tipo   | Requerido | Descripción            |
|--------------------|--------|-----------|------------------------|
| `target-directory` | string | Sí        | Directorio objetivo.   |

#### Outputs
| Nombre          | Descripción                              | Valor                                    |
|-----------------|------------------------------------------|------------------------------------------|
| `build-status`  | `The status of the build process`.       | Output `build-status` del job `deploy`.  |
| `url`           | `The url of the deployed version`.       | Output `url` del job `deploy`.           |

---

#### Añadir un único job: `deploy`
- **Especificaciones:**
  - Debe ejecutarse en `ubuntu-latest`.
  - Define dos outputs:
    - `build-status`, basado en el output del step `build`.
    - `url`, basado en el output del step `deploy`.

#### Steps del job `deploy`:
1. **Checkout repo:**
   - Realiza el checkout del código usando una acción de terceros adecuada.
2. **Build:**
   - **ID:** `build`.
   - Imprime el mensaje: `"Building using directory <valor del input target-directory>"`.
   - Establece el output `build-status` con el valor `"success"`.
3. **Deploy:**
   - **ID:** `deploy`.
   - Imprime el mensaje: `"Deploying build artifacts"`.
   - Establece el output `url` con el valor `https://www.google.com`.

---

### 2. Crear un archivo de flujo de trabajo principal
- **Archivo:** `.github/workflows/18-2-reusable-workflow.yaml`
- **Especificaciones:**
  - **Nombre:** `18 - 2 - Reusable Workflows`
  - **Desencadenantes:** `workflow_dispatch`

#### Añadir jobs al flujo de trabajo:

##### Job 1: `deploy`
- Usa el flujo de trabajo reutilizable definido anteriormente.
- **Key:** `uses`:
  - Valor: `./.github/workflows/18-1-reusable-workflow.yaml`.
- Pasa un valor como argumento de `target-directory` al flujo de trabajo reutilizable.

##### Job 2: `print-outputs`
- **Especificaciones:**
  - Se ejecuta en `ubuntu-latest`.
  - Depende del job `deploy`.
  - Contiene un único step:
    - **Nombre del step:** `Print outputs`.
    - Imprime los siguientes mensajes:
      - `"Build status: [<valor del output build-status>]"`.
      - `"URL: [<valor del output url>]"`.

---

### 3. Confirmar cambios y desencadenar el flujo
1. **Confirmar cambios y hacer push** del código al repositorio.
2. **Desencadenar el flujo de trabajo manualmente** desde la interfaz de usuario.
3. Inspeccionar los resultados de la ejecución del flujo.

---

## Tips

- **Sintaxis importante:**
  - Usa `workflow_call` para definir flujos de trabajo reutilizables.
  - Usa `uses` a nivel de job para invocar flujos de trabajo reutilizables.
  - Usa `outputs` a nivel de job para definir salidas.
  - Usa `needs` a nivel de job para establecer dependencias entre jobs.

