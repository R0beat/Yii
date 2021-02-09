# Yii
## Crear una clase 
```
class public function saludo {
	return "Saludo";
}
```
## Para importar una clase se crea el archivo con el mismo nombre de la clase la incluimos en el controlador
```
Yii::import('application.nomClase');
Yinclude(Yii::getPathOfAlias('application','nomClase.php'));
```
## La utilizamos en el controlador
```
$me = new nomClase;
echo $me -> saludo();
```
# Creamos un modelo 
```
 <?php 
 	class Tabla extends CActiveRecord{ //Importante Extender la classe CActiveRecord 
 		public function tableName(){
 			return 'tabla';
 		}
 		public function rules(){
 			//code
 		}
 	}
 ```
# Creamos el controlador
```
<?php
//Extendemos la Clase Controller
class TablaController extends Controller{
	//
	/* doble // indica la raiz del proyecto */ 
	public $layout ='//layouts/layout';
}
```
# Insertar datos
```
public function create(){
	$model = new Countries(); //Modelo
	if (isset($_POST["Countries"])){
		$model->attributes = $_POST["Countries"]; //Recuperamos los elementos del formulario
		if($model->save()) { //Si los datos se insertan de manera correcta
			Yii::app()->user->setFlash("success"),"Registro guardado exitosamente");
			$this->redirect(array("index"));
		}
		else{
			Yii::app()->user->setFlash("danger"),"Fallo el registro");
		}
	}
	//Renderizamos la vista create, con un arreglo como attibutos el modelo y la variable modelo
	$this->render("create",array("model"=>$model));
}
```
