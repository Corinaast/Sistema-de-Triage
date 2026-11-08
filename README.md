# TriageED — Sistema de triage hospitalario

Aplicación de escritorio en Java que registra pacientes, les asigna un nivel de prioridad según sus síntomas y los atiende en el orden correcto. Todo el comportamiento se apoya en **estructuras de datos implementadas desde cero**: cola con prioridad, lista enlazada y pila.

![Java](https://img.shields.io/badge/Java-21-007396?logo=openjdk&logoColor=white)
![Swing](https://img.shields.io/badge/Interfaz-Swing-5B8C5A)
![Estructuras de datos](https://img.shields.io/badge/Estructuras%20de%20datos-propias-4B5563)

---

## El problema

En una sala de emergencias, el orden de llegada no sirve para decidir a quién atender primero. El sistema clasifica a cada paciente en uno de cinco niveles y garantiza que el más crítico salga siempre primero, sin depender de librerías de colecciones para la lógica principal.

| Nivel | Código | Tiempo de atención | Riesgo |
|---|---|---|---|
| 🔴 Crítico | 1 | Inmediata | Riesgo vital |
| 🟠 Emergencia | 2 | < 15 minutos | Peligro potencial |
| 🟡 Urgente | 3 | < 1 hora | Estado serio |
| 🟢 Poco urgente | 4 | < 2 horas | Condición estable |
| 🔵 No urgente | 5 | Programada | Consulta general |

La prioridad se asigna analizando los síntomas ingresados: el sistema busca palabras clave (`dolor toracico`, `infarto`, `hemorragia severa`, `inconsciencia`, etc.) y asigna el nivel correspondiente.

## Funcionalidades

- **Registro de pacientes** con DNI, nombre, apellido y fecha de nacimiento. El DNI es único: `ListaEnlazadaPacientes.buscarPorDni` evita duplicados.
- **Asignación automática de prioridad** a partir de los síntomas.
- **Atención en orden de prioridad**: `ColaPrioridad.encolar` inserta cada paciente en la posición que le corresponde; `desencolar` siempre devuelve el más crítico.
- **Historial con pila**: cada cambio queda en `PilaHistorial`, lo que permite deshacer la última operación.
- **Cambio manual de prioridad** por DNI.
- **Demostraciones del curso**: ordenamiento por intercambio (método Joyanes), búsqueda recursiva de pacientes críticos y comparación con memoria estática.

## Cómo está construido

```
PriorityTriage/
└── src/
    ├── module-info.java          # módulo PriorityTriage (requiere java.desktop)
    ├── main/Main.java            # arranque de la aplicación
    ├── controller/               # TriageController: casos de uso y coordinación
    ├── model/
    │   ├── Persona.java          # base de Paciente
    │   ├── Paciente.java
    │   ├── NivelPrioridad.java   # enum con código, tiempo y riesgo
    │   ├── ColaPrioridad.java    # cola enlazada ordenada por prioridad
    │   ├── ListaEnlazadaPacientes.java
    │   ├── PilaHistorial.java    # pila para deshacer
    │   ├── NodoPaciente.java
    │   ├── NodoCambio.java
    │   └── SistemaTriage.java    # fachada del dominio
    └── view/TriageView.java      # interfaz Swing
```

La vista no conoce las estructuras: habla con `TriageController`, que a su vez usa `SistemaTriage`. Esa separación permite probar la lógica sin abrir ventanas.

## Cómo ejecutarlo

### Opción 1 — IDE

Abre la carpeta `PriorityTriage` en NetBeans, IntelliJ IDEA o Eclipse y ejecuta la clase `main.Main`.

### Opción 2 — línea de comandos (PowerShell)

```powershell
cd PriorityTriage
javac -d bin (Get-ChildItem -Recurse src -Filter *.java).FullName
java --module-path bin -m PriorityTriage/main.Main
```

En Bash/macOS/Linux:

```bash
cd PriorityTriage
javac -d bin $(find src -name "*.java")
java --module-path bin -m PriorityTriage/main.Main
```

> El repositorio incluye una carpeta `bin/` con clases compiladas de una entrega anterior. Se puede eliminar sin afectar al código fuente.

## Documentación

- [`Presentación - TriageED - Sistema de Triage.pdf`](Presentación%20-%20TriageED%20-%20Sistema%20de%20Triage.pdf) — explicación del diseño y de las estructuras usadas.
- [`U2Lab1Grupo1.pdf`](U2Lab1Grupo1.pdf) — enunciado y rúbrica del laboratorio.

## Autora

**Corina Acosta** — [@Corinaast](https://github.com/Corinaast)

---

Proyecto académico de la asignatura de **Estructuras de Datos** (ESPE). La asignación de prioridad por palabras clave es una simplificación didáctica y **no sustituye** un protocolo clínico real.
