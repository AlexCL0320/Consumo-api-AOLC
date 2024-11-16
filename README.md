# ConsumoApiAOLC_INICIALES - Alexis Oswaldo Lopez Carreño

Proyecto Generado con [Angular CLI](https://github.com/angular/angular-cli) version 18.2.10.

## Explicacion de la Practica 

Para comenzar  con el desarrollo de la practica es necesario contar con algunos  requisitos de instalacioón previos a la ejecución del repositorio

### Requisitos Previos
1.- IDE de desarrollo (VS CODE)

2.- Instalación del Framework ANGULAR CLI vs 18.2.10

3.- Instalación de librerias npm

4.- Instalación de librerias boostrap

5.- Instalación de libreria ngx pagination

Para poder ejecutar de forma correcta el contenido del repositorio en tu computador es necesario que una vez clonado el repositorio en tu area de trabajo se realice la instalción de las librerias anteriormente mencionadas a traves de tu entorno de desarrollo (IDE).

## Desarollo de la Practica 

### 1.- Creación del Proyecto
Para comenzar a trabar el consumo de APIS web será necesaria la creación de un proyecto en ANGULAR a traves del cual se pueda consultar el contenido de una API haciendo uso de servicios; para finalmente presentar en forma de lista con paginación todo el contenido extraido de la API.

Por ello para la generación de un nuevo proyecto en Angular debemos abrir la terminal o consola, dentro de ella ubicarno en el directorio donde deseemos crear nuestro poryecto y ejecutar los siguientes comandos:

•	ng new consumo-api-INICIALES --routing --standalone=false

•	Ingresa en la carpeta del proyecto:

•	cd consumo-api-INICIALES

•	aceptar las opciones predeterminadas cuando se solicite.

![image](https://github.com/user-attachments/assets/a5eeb5c3-be5d-4212-8546-d4e3a253e69a)
Comandos de Creación 

![image](https://github.com/user-attachments/assets/1cc12a1c-c266-497a-9aae-db6fc2a1cf5b)
Proyecto Base Generado


### 2.-Generación del Servicio para Consumo de API
Para iniciar el consumo de API externas es necesarios la configuración de servicios dentro de nuestro proyecto a traves de los cuales se pueda accesar a la API y recupear su contenido a traves de un JSON.

Por ello dentro del proyecto se debe generar un servicio en la carpeta services que se encargue de consumir la API Web; para ello en la terminal debemos escribir: 

• ng generate service services/user

![image](https://github.com/user-attachments/assets/b1e335c6-1864-4b75-85b9-dfd81b83aba5)
Comandos de Generación del Servicio

### 3.- Configurar Modelo de Servicio (HttpCLienteModule)
Para permitir el acceeso del servicio dentro de los compoentes de nuestro proyecto es necesario configurar las importaciones y rutas necesarias dentro del modulo de nuestro aplicacion, debemos asegurarnos de importar la libreria HttpClienteModule y definir a ruta a nuestro servicio con la instrucción import { UserService } from './services/user.service'. Para confiugrar la paginación en nuestra proyecto será necesario contemplar la importación de la libreria NgxPaginationModule

Condigo de Configuración de Acceso al Servicio

    import { BrowserModule } from '@angular/platform-browser';
    import { NgModule } from '@angular/core';
    import { HttpClientModule } from '@angular/common/http';
    import { AppComponent } from './app.component';
    import { UserService } from './services/user.service';
    import { UserListComponent } from './user-list/user-list.component';
    import { NgxPaginationModule } from 'ngx-pagination';
    
    @NgModule({
    declarations: [
    AppComponent,
    UserListComponent
    ],
    
    imports: [
    BrowserModule,
    HttpClientModule, NgxPaginationModule
    ],
    
    providers: [UserService],
    bootstrap: [AppComponent]
    })
    
    export class AppModule { }

![image](https://github.com/user-attachments/assets/659e747e-ee72-4e7f-84cb-4c219e3f7eaf)

Ejemplo de Condiguración

### 4.- Creación del Componente de Tabla de Usuarios
La presentación de los datos consumidos a traves de la API se realziara a traves de la presentación de una tabla estándar con paginación; por esto se debe crear un nuevo componente del proyecto que se encargue de manejar el contenido html y operaciones TypeScript

Para ello generaremos un nuevo componente llamado user-list a traves del siguiente comando
• ng generate component user-list

![image](https://github.com/user-attachments/assets/052bec94-220d-4814-9601-cc0f15f6599c)
Comando de Generación de Componente user-list


### 5.- Actualización de TypeSccript del Componente user-list para la Presentación de la API
Finalizada la creación del componente debemos dirigirnos al archivo user-list.component.ts para cconfigurar la logica de acceso al servicio del consumo de la API y el como se reccorrera el JSON de datos para su presentación en el HTML.

Para esto del archivo user-list.component.tx copiamos el siguiente código:

    import { Component, OnInit } from '@angular/core';
    import { UserService } from '../services/user.service';
    @Component({
      selector: 'app-user-list',
      templateUrl: './user-list.component.html',
      styleUrls: ['./user-list.component.css']
    })
    
    export class UserListComponent implements OnInit {
      users: any[] = [];
      page: number = 1; // Página inicial
      constructor(private userService: UserService) {}
      ngOnInit(): void {
        this.userService.getUsers().subscribe(data => {
          this.users = data;
        });
      }
    }


![image](https://github.com/user-attachments/assets/1bf032e4-6d1a-4d34-b181-927d6a015ddd)
Ejemplo Modificacion del TS

### 6.- Modificacion del Archivo de Vista (html) del Componente user-list para la Presentación de la API
Terminada la modificacion del archivo TypeScript para la recuperacion de los datos debemos proceder a la edicion del archivo user-list.component.html para inidicarle a la apliacion el como deseamos presentar nuestros datos. Para este ejemplos los datos serán presetado mediante una tabla basica donde se almacene el id, nombre, correo y rol de cada registro devuelto en el JSON.

Para ello abimos el archivo Abrir src/app/user-list/user-list.component.html y agregaamos el
siguiente código para crear una tabla donde se mostrarán los datos de los usuarios

    <div class="container mt-5">
        <div class="card shadow-sm">
            <div class="card-header bg-danger text-white">
                <h4 class="mb-0">Lista de Usuarios</h4>
            </div>
            <div class="card-body p-0">
                <table class="table table-hover table-bordered mb-0">
                    <thead class="thead-dark">
                        <tr>
                            <th scope="col">ID</th>
                            <th scope="col">Nombre</th>
                            <th scope="col">Correo</th>
                            <th scope="col">Rol</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr *ngFor="let user of users | paginate: { itemsPerPage: 10, currentPage: page }">
                            <td>{{ user.id }}</td>
                            <td>{{ user.name }}</td>
                            <td>{{ user.email }}</td>
                            <td>{{ user.role }}</td>
                        </tr>
                    </tbody>
                </table>
            </div>
            <!-- Controles de paginación -->
            <div class="card-footer text-center">
                <pagination-controls (pageChange)="page = $event"></pagination-controls>
            </div>
        </div>
    </div>

Ejemplo Modificación del Archivo HTML

### 6.- Integración del Component en la Apliacion
Por ultimo cuando se termina la configuración del consumo y presentación del contenido de la API el ultimo paso será definir la ruta de acceso del compoente dentro del arhivo src/app/app.module.ts agregando la importación 

    import { UserListComponent } from './user-list/user-list.component';

Configurada el manejo de la ruta a nuestro compoenente nos dirijimos a nuestro archivo src/app.component.html y dentro compiamos la etiqueta de nuestro componente user-list para mostrar al usuario final la tabla con los datos consultados a traves de la API Web.

Instrucción de incorportaccion:

    <app-user-list></app-user-list>

![image](https://github.com/user-attachments/assets/58c6fb88-5ec4-4dab-a963-872b7adc75a3)

Ejemplo de Incorporación del Componente user-list

### 6.- Ajecución de la Aplicacion y Comporbación del Consumo de la API
Con esto hemos finalizado el desarrollo de la pracica; para verificar que hemos realizado correctamente toda la configuracion del consumo de la API y que los datos del JSON estan siendo recuperados de manera efectiva debemos ejecutar el proyecto a tra ves del comando de ejecución e ingresar a la ruta que se nos despliega para validar que efectivamente se presentan los datos.

Comando de ejecución:
    
    ng serve 

![image](https://github.com/user-attachments/assets/42aec79c-bf45-458c-9d75-84a0d67c183e)
Ejecución de la Apliacion 

# Resultado Final del Consumo de la API
![image](https://github.com/user-attachments/assets/4aedd154-6649-466f-b35e-8a736e167125)

# Pregunta de Apoyo

## ¿Qué hace el método getUsers en este servicio?
El método getUsers en el servicio UserService tiene la función de consumir la API pública especificada (https://api.escuelajs.co/api/v1/users) y obtener la lista de usuarios. Para ello, utiliza el método get del módulo HttpClient de Angular, que permite realizar solicitudes HTTP.

El método está definido de forma que devuelve un Observable de tipo any[], lo que significa que la respuesta será un conjunto de datos que pueden ser manejados de forma asíncrona por el componente que llame a este servicio. Es decir, getUsers se encarga de hacer una petición GET a la URL especificada (API) y devuelve la respuesta en forma de un Observable que contiene un array de objetos representando los usuarios.

## ¿Por qué es necesario importar HttpClientModule?
Es necesario importar HttpClientModule en Angular porque este módulo proporciona la funcionalidad requerida para que la aplicación pueda realizar peticiones HTTP y consumir datos de APIs externas. HttpClientModule habilita el uso del servicio HttpClient, que permite enviar solicitudes HTTP (GET, POST, PUT, DELETE, etc.) y manejar las respuestas de manera eficiente.

Sin HttpClientModule, Angular no tiene acceso al servicio HttpClient, lo que significa que no se pueden realizar solicitudes HTTP desde la aplicación

## ¿Qué función cumple el método ngOnInit en el componente UserListComponent?
El método ngOnInit en el componente UserListComponent cumple la función de inicializar la lógica que debe ejecutarse cuando el componente se carga por primera vez; ngOnInit funciona como un método de ciclo de vida en Angular que se ejecuta automáticamente después de que Angular ha configurado todas las propiedades del componente y las ha inicializado.
En este caso, ngOnInit se utiliza para hacer una llamada al método getUsers del servicio UserService tan pronto como el componente se carga y devolver un Observable que se suscribe dentro de ngOnInit para manejar la respuesta de forma asíncrona. Cuando se reciben los datos, se asignan a la propiedad users, lo que permite que la lista de usuarios se muestre en la plantilla de nuestro componente.


## ¿Para qué sirve el bucle *ngFor en Angular?
El bucle *ngFor en Angular es una directiva estructural que permite iterar sobre una colección de elementos y renderizarlos en la vista de forma dinámica. Se utiliza para repetir un bloque de código HTML por cada elemento de un array, generando una lista de elementos en el DOM.

En este ejemplo específico, *ngFor="let user of users" sirve para recorrer la colección users, que es un array de objetos de usuarios cargados en el componente UserListComponent. Por cada elemento en el array users, se genera una fila <tr> en la tabla, y cada celda <td> muestra una propiedad específica del objeto user (id, name, email, role). Esto permite mostrar automáticamente la lista de usuarios obtenida de la API en la interfaz de manera organizada y legible.

# Preguntas de Reflexión

## Qué ventajas tiene el uso de servicios en Angular para el consumo de APIs?
El uso de servicios en Angular para consumir APIs ofrece varias ventajas clave:

Reutilización del código: Al centralizar la lógica de las peticiones HTTP en un servicio, este puede ser reutilizado en múltiples componentes. Esto evita la repetición de código y facilita su mantenimiento.

Desacoplamiento: El servicio actúa como una capa intermedia entre el componente y la API, lo que desacopla la lógica de negocio de la lógica de presentación. Esto hace que el código sea más limpio y fácil de entender.

Manejo eficiente de peticiones asíncronas: Angular, a través de los servicios y el HttpClient, permite manejar peticiones HTTP de manera asíncrona con observables

## ¿Por qué es importante separar la lógica de negocio de la lógica de presentación?
Separar la lógica de negocio de la lógica de presentación es fundamental porque nos permite tener:

Mantenibilidad: Permite que el código sea más fácil de mantener. Ya que si la lógica de negocio está separada, los cambios en los requerimientos de la aplicación o la lógica de negocio no afectan directamente a la presentación o interfaz de usuario, y viceversa.

Escalabilidad: A medida que el proyecto crece, esta separación hace que sea más sencillo añadir nuevas funcionalidades sin crear dependencias entre el diseño visual y la lógica de la aplicación.

Reutilización: La lógica de negocio, al estar en servicios o en una capa independiente, puede ser reutilizada por diferentes componentes o incluso en otras aplicaciones sin necesidad de duplicar código.

## ¿Qué otros tipos de datos o APIs podrías integrar en un proyecto como este?
En proyecto similares al que trabajamos a lo largo del desarrollo de la practica se podrian integrar apis como:

APIs de autenticación: APIs que permitan la autenticación y autorización de usuarios (por ejemplo, utilizando JWT o servicios de terceros como OAuth).

APIs de productos: Si el proyecto es una aplicación de comercio electrónico o inventarios, se podrían integrar APIs para obtener información sobre productos, categorías, precios, etc.

APIs de geolocalización: Para obtener la ubicación del usuario o mostrar mapas interactivos, APIs como Google Maps o Mapbox podrían integrarse.
``
APIs de pagos: Si el proyecto implica transacciones financieras, se podrían utilizar APIs de pago como Stripe, PayPal o similares para procesar pagos.



