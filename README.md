# FRAMEWORK YII 1
## Crear una clase 👨‍💻
```
class public function saludo {
	return "Saludo";
}
```
## Para importar una clase se crea el archivo con el mismo nombre de la clase la incluimos en el controlador 👨‍💻
```
Yii::import('application.nomClase');
Yinclude(Yii::getPathOfAlias('application','nomClase.php'));
```
## La utilizamos en el controlador 👨‍💻
```
$me = new nomClase;
echo $me -> saludo();
```
## Creamos un modelo 👨‍💻
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
# 👁️👁️ LOS CONTROLADORES TIENE ACCIONES (SON MÈTODOS CON EL PREFIJO ACTION PUEDEN RECIBIR O NO RECIBIR PARAMETROS)
## Creamos el controlador
```
<?php
//Extendemos la Clase Controller
class TablaController extends Controller{
	//
	/* doble // indica la raiz del proyecto */ 
	public $layout ='//layouts/layout';
}
```
## Insertar datos ➕
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
## Actualizando Registros 🔄
```
	//a la función le pasamos el atributo $id para recuperar el registro a actualizar
	public function actionUpdate($id){
		//Consultamos el registro por su id 
		//CActiveRecord nos permite consultar un registro por su llave primaraia a travez de findByPk($id)
		$model=Countries::model()->findByPk($id);
		//Determinamos que la variable este definida
		if (isset($_POST["Countries"])){
			$model->atributes=$_POST["Countries"];
			if($model->save()){
				Yii::app()->user->setFlash("success"),"Registro actualizado exitosamente");
			}
			else{
				Yii::app()->user->setFlash("danger"),"Error al actualizar");
			}
		}
	}
```
## Eliminar Registros  ❌
```
	public function actionDelete($id){
		//Consultamos el registro por su id 
		$model=Countries::model()->deleteByPk($id);
		Yii::app()->user->setFlash("success"),"Registro Borrado exitosamente");
	}
```
## Deshabilitar 🚫
```
	public function actionEnable($id){
		//Consultamos el registro por su id 
		$model=Countries::model()->findByPk($id);
		if($model->satus==1){
			$model->satus=0;
		}
		else{
			$model->status=1;
		}
		$model->save();
		$this->redirect(array("index")); //Nos redirecciona a la vista index
	}
```
## Path Alias 📂
```
echo Yii::getPathOfAlias('application') //📂protected
echo Yii::getPathOfAlias('webroot') //📂root directorio principal
echo Yii::getPathOfAlias('ext') //📂protected📂extension
echo Yii::getPathOfAlias('zii') //📂framework📂zii
```
_En el archivo de configuracin main descomentamos esta linea para crear nuesttros propios alias_
```
// uncomment the following to define a path alias
	 Yii::setPathOfAlias('local','path/to/local-folder');
```
_Creamos un alias para nuestro directorio_
```
	Yii::setPathOfAlias('nombre',dirname(__FILENAME__)."/PATH");
```
## Components 📋
```
// application components
    'components' => array(
    	'nuevo_componente'=>array(
    		"class"=>"ext.GComponente",
    	),
        'user' => array(
            // enable cookie-based authentication
            'allowAutoLogin' => true,
            'loginRequiredAjaxResponse' => 'YII_LOGIN_REQUIRED',
        ),
```
_Creamos el componente  "📂protected📂extension📂NombreComponente.php"_
```
<?php
//Para crear un componente extendemos la clase CAplicationsComponents
	class Componente extends CAplicationsComponents{
		//Declaramos las funciones que queremos que se ejecuten
		public function init(){
			//Esta Acción se ejecutara la primera vez que utilicemos el componente
			echo "Inicializando";
		}
		public function accion(){
			return "Accion";
		}
	}
```
_Accedemos al componete desde el controlador_
```
<?php
//Extendemos la Clase Controller
	class TablaController extends Controller{
		public function actionIndex(){
			echo Yii::app()->alias->accion();
		}
	}
```

