Laboratorio HttpClient Angular
Crearemos una aplicación con el AngularCLI, en la consola escribimos ng new angular-http
Corremos el siguiente comando para crear nuestro servicio ng generate service app
En nuestro módulo principal importamos el módulo de HttpClientModule
import { HttpClientModule } from '@angular/common/http';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    HttpClientModule,
  ],
  providers: [],
  bootstrap: [AppComponent]
})
Importamos la clase HttpClient en nuestro app.service.ts
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';

@Injectable({
  providedIn: 'root'
})
export class AppService {

  constructor(private _http: HttpClient) { }
}
Ahora creamos una interfaz de typescript en una carpeta de models, lo llamamos answer.model.ts
export interface Answer {
  answer: string,
  forced: boolean,
  image: string
}
Creamos una función que retornará la respuesta de la API en nuestro servicio app.service.ts
  obtenerRespuesta() {
    return this._http.get<Answer>(this.apiUrl);
  }
Importamos el servicio en nuestro componente
import { Component } from '@angular/core';
import { AppService } from './app.service';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss']
})
export class AppComponent {
  constructor(private appService: AppService) {
    
  }
}
En nuestro componente creamos una variable llamada respuesta de tipo Answer y otra llamada pregunta de tipo string public pregunta = ''; public respuesta?: Answer;
Creamos un método llamado onSubmit en el componente, el cuál se suscribirá al servicio obtenerRespuesta
  onSubmit() {
    this.appService.obtenerRespuesta().subscribe(res => this.respuesta = res);
  }
En nuetro módulo principal importamos el módulo de FormsModule, para poder hacer uso del NgModel
Ahora en nuestro HTML crearemos el siguiente componente
<h1>Realiza una pregunta de Si o No</h1>

<input type="text" placeholder="¿Qué deseas preguntar?" [(ngModel)]="pregunta" >

<button (click)="onSubmit()">Pregunta</button>

<div style="margin-top: 20px;">
  <h2>{{respuesta?.answer}}</h2>

  <ng-container *ngIf="respuesta">
    <img *ngIf="respuesta.image" [src]="respuesta.image" alt="">
  </ng-container>
</div>
Corremos nuestra aplicación con un ng serve
