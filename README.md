
// yield
// middleware
// crfs
// scope
// fillable
// store --> se validan los datos del formulario
// asignación de clave Hash
// distintas formas de almacenarse, cookies y/o sesiones


### Instalar composer --> getcomposer.org
Tener actualizado php versión mínima 7.0
powershell // ssl puerto 443->4430 // php artisan serve // opt/lampp lamp.start
sudo dpkg -i paquete.deb // service mysql stop

### Creación de proyecto
composer create-project --prefer-dist laravel/laravel proyecto / permisos
cp server.php index.php

* Idioma
composer require laraveles/spanish
php artisan laraveles:install-lang

* Migraciones y creación de modelos, controladores (resource para crear esqueleto CRUD)
php artisan make:migration create_alumnos_table
php artisan make:model Alumno -m
php artisan make:controller AlumnosConstroller --resource
php artisan make:auth
php artisan migrate

* Laravel Collective
composer require ranabd36/laravelcollective
composer require "laravelcollective/html":"^5.5”

Tipo de dato-->enum('tipo',['normal','administrador'])->default('normal');

index.php
header('Location: public/index.php');

routes --> web.php
Route::resource('alumnos','AlumnosController'); (resource para todo);
Route::get('/home', 'AlumnosController@index')->name('home');
// Enlace llamada en home.blade: <a  class="btn btn-warning" href="{{route('alumnos.destroy','borrar/'.$alumno->id)}}">Borrar</a>
Route::get('alumnos/borrar/{id}', 'AlumnosController@destroy')->name('alumnos.borrar');
// Enlace llamada <a  class="btn btn-info" href="{{route('alumnos.show',$alumno->id)}}">Mostrar</a>
Route::get('alumnos/show/{id}','AlumnosController@show')->name('alumnos.show');


-- Cookies: en donde queramos mostrarlo
@php
    echo "Ultimo acceso".session('acceso');

    // cookies setcookie(nombre, contenido,expira);
    $expira=time()+60*60*24*365; //1 año
    if (!isset($_COOKIE['acceso'])){
        setcookie('acceso',1,$expira);
    }else{
        setcookie('acceso',$_COOKIE['acceso']+1,$expira);
    } 
    echo "<br>Vistitas en el año: ".$_COOKIE['acceso'];
@endphp


-- Session: en ControllerLogin
        $acceso = date('d/m/Y-h:i:s');
        session(['acceso' => $acceso]);



 //show
 $alumno=Alumno::find($id);
 return view('alumnos.show')->with('alumno',$alumno);

 //delete
 $alumno = Alumno::find($id);
 $alumno->delete();          
 return redirect()->route('alumnos.index');


// Formulario con Laravel Collective
{!! Form::open(['method' => 'POST', 'route' => ['alumnos.borrar']]) !!}
    <button class="btn btn-danger" type="submit" title='Borrar' onclick="return confirm('¿Seguro que desea borrar   alumno?')"> Borrar
    </button>
    <input type="hidden" name="id" value="{{$alumno->id}}">
{!! Form::close() !!}

--> Route::post('/alumnos-borrar','AlumnosController@borrar')->name('alumnos.borrar');
--> $alumno = Alumno::findOrFail($request->id); $alumno->delete(); return redirect()->route('home');
