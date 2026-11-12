# Historial de Vacunas – Clínica Veterinaria (Ruby)

Programa en **Ruby** que lleva el historial de vacunas de una clínica veterinaria. Fue desarrollado como actividad de Programación Orientada a Objetos para evidenciar herencia, clases abstractas, encapsulamiento, asociación entre clases, polimorfismo, módulos, modularidad y manejo de archivos.

---

## Descripción del caso

Las clínicas veterinarias atienden a muchas mascotas de distintos propietarios, y cada vacuna aplicada debe quedar en el historial del animal. Cuando ese historial se lleva en papel, se pierden registros, se repiten vacunas y se aplican dosis a mascotas cuyo propietario no es cliente del veterinario que las atiende.

Este programa resuelve ese problema registrando cada vacuna, validando las reglas de la clínica y guardando el historial en un archivo CSV.

---

## Estructura del proyecto

```
clinica-veterinaria-ruby/
├── main.rb                     # Programa principal: datos de prueba y demostración
├── persona.rb                  # Clase base (abstracta simulada)
├── veterinario.rb              # Veterinario < Persona
├── propietario.rb              # Propietario < Persona
├── mascota.rb                  # Mascota (incluye Comparable)
├── historial_veterinaria.csv   # Historial de vacunas (se genera al ejecutar)
└── README.md                   # Este archivo
```

| Archivo | Qué contiene |
|---|---|
| `persona.rb` | Clase `Persona` con `@nombre`, `@documento`, `to_s` e `identificacion_individuo`. No se puede instanciar directamente. |
| `veterinario.rb` | Clase `Veterinario`: tarjeta profesional, especialidad, lista de propietarios y el método `vacunar`. |
| `propietario.rb` | Clase `Propietario`: teléfono, lista de mascotas y listado de mascotas por edad. |
| `mascota.rb` | Clase `Mascota`: nombre, especie, edad, vacunas aplicadas y contador de mascotas creadas. |
| `main.rb` | Carga todas las clases con `require_relative`, crea los datos de prueba, demuestra cada regla y maneja el CSV. |

---

### Datos de prueba incluidos

| Rol | Nombre | Detalle |
|---|---|---|
| Veterinario | John Dolittle | Animales exóticos, atiende a Shrek |
| Veterinario | Ellie Sattler | Aves y reptiles, atiende a Indiana Jones |
| Propietario | Shrek | Dueño de Pulgoso Balboa, Gatúbela y Loro Skywalker |
| Propietario | Indiana Jones | Dueño de Cuacsparrow y Miauricio |

| Mascota | Especie | Edad |
|---|---|---|
| Pulgoso Balboa | perro | 10 años |
| Gatúbela | gato | 7 años |
| Loro Skywalker | ave | 15 años |
| Cuacsparrow (un ganso) | ave | 5 años |
| Miauricio | gato | 2 años |

### Rechazos que demuestra el programa

| Situación | Mensaje que verás |
|---|---|
| Agregar un propietario `nil` | `[RECHAZADO] ... no se puede agregar un propietario nulo.` |
| Agregar un propietario duplicado | `[RECHAZADO] ... el propietario Shrek ya está en su lista.` |
| Vacunar a la mascota de un propietario que el veterinario no atiende | `[RECHAZADO] ... no atiende a ese propietario ...` |
| Vacunar una mascota que no pertenece al propietario indicado | `[RECHAZADO] Miauricio no pertenece a Shrek.` |
| Aplicar dos veces la misma vacuna | `[RECHAZADO] Pulgoso Balboa ya recibió la vacuna 'Rabia'.` |
| Crear una mascota con especie no permitida | `[ArgumentError] Especie 'llama' no válida. Solo se permite: perro, gato, ave` |

### Probar con tus propios datos

Para registrar otras mascotas o vacunas, edita la sección de datos de prueba en `main.rb`. Por ejemplo:

```ruby
# Crear una mascota nueva (la especie debe ser perro, gato o ave)
nueva = Mascota.new('Firulais', 'perro', 3)

# Asignarla a un propietario
shrek.agregar_mascota(nueva)

# Vacunarla con un veterinario que atienda a ese propietario
dolittle.vacunar(nueva, shrek, 'Rabia')
```

Al volver a ejecutar `ruby main.rb`, el archivo `historial_veterinaria.csv` se reescribe con el historial actualizado.

---

## Reglas de negocio

- El veterinario agrega propietarios a su lista y **rechaza `nil` y duplicados**.
- Solo puede vacunar mascotas de propietarios que él atiende.
- Solo puede vacunar una mascota que pertenezca realmente a ese propietario.
- Una mascota **no puede recibir dos veces la misma vacuna**.
- La especie solo puede ser **perro, gato o ave**; cualquier otro valor lanza `ArgumentError`.
- Cada rechazo se informa en consola con un mensaje claro.

---

## Conceptos de POO aplicados

| Concepto | Dónde se aplica |
|---|---|
| **Herencia** | `Veterinario < Persona` y `Propietario < Persona`, ambos llaman a `super` en su `initialize`. |
| **Clase abstracta (simulada)** | `Persona` lanza `NotImplementedError` si se intenta instanciar directamente o si una hija no sobrescribe `identificacion_individuo`. |
| **Polimorfismo** | Un arreglo con veterinarios y propietarios se recorre invocando `identificacion_individuo`; cada objeto responde a su manera. |
| **Encapsulamiento** | `attr_reader` para solo lectura; listas internas devueltas con `dup.freeze`; `agregar_vacuna` es privado y solo se usa a través del veterinario. |
| **Asociación entre clases** | El veterinario tiene propietarios, el propietario tiene mascotas y la mascota tiene vacunas. |
| **Módulos (mixins)** | `Mascota` incluye `Comparable` y define `<=>` para comparar por edad, lo que permite usar `sort` y `max`. |
| **Variable y método de clase** | `@@total_registradas` y `Mascota.total_registradas` cuentan las mascotas creadas. |
| **Reutilización de código** | `Persona` concentra nombre, documento y `to_s`, que las clases hijas reutilizan. |
| **Modularidad** | Un archivo por clase, cargados desde `main.rb` con `require_relative`. |
| **Manejo de archivos** | Lectura y escritura del CSV con la librería `csv`. |
| **Manejo de excepciones** | `begin/rescue` cuando el archivo no existe y para los errores demostrados en consola. |

---

## Formato del archivo CSV

El historial se guarda en `historial_veterinaria.csv`, **una línea por vacuna**, sin encabezado:

```
mascota,especie,propietario,vacuna
```

Ejemplo:

```
Pulgoso Balboa,perro,Shrek,Parvovirus
Pulgoso Balboa,perro,Shrek,Rabia
Gatúbela,gato,Shrek,Triple felina
Loro Skywalker,ave,Shrek,Newcastle
Cuacsparrow,ave,Indiana Jones,Newcastle
Miauricio,gato,Indiana Jones,Leucemia felina
```

El programa lee ese archivo y muestra cuántas vacunas se aplicaron por especie. Si el archivo no existe, el error se captura con `begin/rescue` y se muestra el mensaje `[ERROR] El archivo '...' no existe.` sin detener el programa.

## Evidencias

<img width="1112" height="875" alt="Screenshot 2026-11-11 201748" src="https://github.com/user-attachments/assets/548c653b-5456-4836-8c2b-283b376ee080" />
<img width="1411" height="892" alt="Screenshot 2026-11-11 201758" src="https://github.com/user-attachments/assets/7f782d9f-235f-4188-a2c8-76322a55ffe8" />
<img width="1378" height="894" alt="Screenshot 2026-11-11 201807" src="https://github.com/user-attachments/assets/54b42c92-0570-4305-a517-3a7c3a655624" />
