## Código HTML
```html
<div>
  <form [formGroup]="form" (submit)="send()">
    <!-- Input email -->
    <div>
      <label for="email">Email</label>
      <input type="email" id="email" formControlName="email">
    </div>

    <!-- Input password -->
    <div>
      <label for="password">Contraseña</label>
      <input type="password" id="password" formControlName="password">
    </div>

    <!-- Boton enviar -->
    <div>
      <button [disabled]="form.invalid">Enviar</button>
    </div>

  </form>
</div>

<div>
  Valido: {{form.valid | json}}
</div>
```

### Explicación

#### Contenedor del formulario
```html
<div>
  <form [formGroup]="form" (submit)="send()">
```
- **[formGroup]="form"**: Esta directiva vincula el formulario HTML con una instancia de `FormGroup` en tu componente TypeScript. Esto significa que los controles del formulario estarán gestionados por el modelo de datos reactivo definido en el componente.
- **(submit)="send()"**: Maneja el evento de envío del formulario y llama a la función `send()` en tu componente cuando el formulario se envía.

#### Campo de entrada para el email
```html
<div>
  <label for="email">Email</label>
  <input type="email" id="email" formControlName="email">
</div>
```
- **Label**: Etiqueta para el campo de entrada del correo electrónico.
- **Input**: Campo de entrada para el correo electrónico.
  - **type="email"**: Define el tipo de entrada como correo electrónico, proporcionando validación básica del navegador.
  - **formControlName="email"**: Vincula el control del formulario con el `FormControl` correspondiente en el `FormGroup` definido en el componente.

#### Campo de entrada para la contraseña
```html
<div>
  <label for="password">Contraseña</label>
  <input type="password" id="password" formControlName="password">
</div>
```
- **Label**: Etiqueta para el campo de entrada de la contraseña.
- **Input**: Campo de entrada para la contraseña.
  - **type="password"**: Define el tipo de entrada como contraseña, ocultando el texto introducido.
  - **formControlName="password"**: Vincula el control del formulario con el `FormControl` correspondiente en el `FormGroup` definido en el componente.

#### Checkbox para términos y condiciones
```html
<div>
  <label for="terms">
    <input type="checkbox" id="terms" formControlName="terms">
    Acepto los términos y condiciones
  </label>
</div>
```
- **Label**: Etiqueta para el checkbox de términos y condiciones.
- **Input**: Campo de entrada tipo checkbox.
  - **type="checkbox"**: Define el tipo de entrada como checkbox.
  - **formControlName="terms"**: Vincula el control del formulario con el `FormControl` correspondiente en el `FormGroup`.


#### Botón de Envío
```html
<div>
  <button [disabled]="form.invalid">Enviar</button>
</div>
```
- **<button [disabled]="form.invalid">Enviar</button>**: Botón de envío del formulario.
  - **[disabled]="form.invalid"**: Deshabilita el botón si el formulario es inválido, evitando que el usuario lo envíe hasta que todos los controles sean válidos.

#### Visualización del estado de validez
```html
<div>
  Valido: {{form.valid | json}}
</div>
```
- **Valido: {{form.valid | json}}**: Muestra el estado de validez del formulario en formato JSON. Esto es útil para depurar y verificar que las validaciones funcionan correctamente.

### Resumen

Este formulario reactivo en Angular gestiona los campos de entrada para el correo electrónico y la contraseña, asegurando que solo se pueda enviar cuando todos los campos sean válidos. Además, utiliza `formControlName` para enlazar los campos del formulario con el modelo de datos en el componente TypeScript.


## Código TS

### Importaciones
```typescript
import { CommonModule } from '@angular/common';
import { Component } from '@angular/core';
import { FormBuilder, FormGroup, FormsModule, ReactiveFormsModule, Validators } from '@angular/forms';
```
- **CommonModule**: Proporciona directivas comunes de Angular (como `ngIf` y `ngFor`).
- **Component**: Decorador de Angular para definir un componente.
- **FormBuilder, FormGroup, FormsModule, ReactiveFormsModule, Validators**: Importaciones necesarias para trabajar con formularios reactivos en Angular.

### Decorador del componente
```typescript
@Component({
  selector: 'app-root',
  imports: [FormsModule, CommonModule, ReactiveFormsModule],
  templateUrl: './app.component.html',
  styleUrl: './app.component.css'
})
```
- **selector**: Define el selector CSS que identifica este componente en una plantilla.
- **imports**: Lista de módulos adicionales que este componente necesita.
- **templateUrl**: Ruta al archivo HTML que contiene la plantilla del componente.
- **styleUrl**: Ruta al archivo CSS que contiene los estilos del componente.

### Clase del componente
```typescript
export class AppComponent {
  title = 'formulario';

  public form!: FormGroup;

  constructor(private formBuilder: FormBuilder) {}

  ngOnInit(): void {
    this.form = this.formBuilder.group({
      email: ['', [
        Validators.required,
        Validators.email
      ]],
      password: ['', [
        Validators.required,
        Validators.minLength(6)
      ]],
      terms: [false, [
        Validators.requiredTrue
      ]]
    });
  }

  send(): any {
    console.log(this.form.value);
  }
}
```
#### Propiedades
- **title**: Propiedad que contiene el título del componente.
- **form**: Propiedad que contiene la instancia de `FormGroup`, representando el formulario reactivo.

#### Constructor
```typescript
constructor(private formBuilder: FormBuilder) {}
```
- **formBuilder**: Inyección de dependencia de `FormBuilder`, utilizado para construir el `FormGroup`.

#### Método `ngOnInit`
```typescript
ngOnInit(): void {
  this.form = this.formBuilder.group({
    email: ['', [
      Validators.required,
      Validators.email
    ]],
    password: ['', [
      Validators.required,
      Validators.minLength(6)
    ]],
    terms: [false, [
      Validators.requiredTrue
    ]]
  });
}
```
- **ngOnInit()**: Método del ciclo de vida del componente que se ejecuta al inicializar el componente.
- **this.formBuilder.group({...})**: Crea un `FormGroup` que contiene los controles del formulario.
  - **email**: Control con validadores `required` y `email`.
  - **password**: Control con validadores `required` y `minLength(6)`.
  - **terms**: Control tipo booleano con validador `requiredTrue`.

#### Método `send`
```typescript
send(): any {
  console.log(this.form.value);
}
```
- **send()**: Método que se ejecuta al enviar el formulario. Imprime en la consola los valores actuales del formulario.

### Resumen
Este código define un componente Angular que gestiona un formulario reactivo con campos para email, contraseña y aceptación de términos. El formulario incluye validaciones para asegurar que los datos introducidos cumplen ciertos criterios antes de permitir el envío.

