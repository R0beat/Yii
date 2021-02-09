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
## 👁️👁️ LOS CONTROLADORES TIENE ACCIONES (SON MÉTODOS CON EL PREFIJO ACTION PUEDEN RECIBIR O NO RECIBIR PARAMETROS)
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
//Para crear un componente extendemos la clase CAplicationsComponent
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
_Accedemos al componete desde el controlador_ 💻
```
<?php
//Extendemos la Clase Controller
	class TablaController extends Controller{
		public function actionIndex(){
			echo Yii::app()->alias->accion();
		}
	}
```
_component request_
```
		echo Yii::app()->request->baseUrl.'<br>'; //📂root
		echo Yii::app()->request->requestUri.'<br>';📂path_actual
		echo Yii::app()->request->pathInfo.'<br>';📂carpeta_actual
		echo Yii::app()->request->urlReferrer.'<br>';
		echo Yii::app()->request->queryString.'<br>';📂Muestra en uns string una consulta
```
# Exportar a Excel 📗
```
<?php
//Extendemos la Clase Controller
	class TablaController extends Controller{
		public function actionIndex(){
			$model =Countries::model()->findAll();
			$content=$this->renderPartial("vista",array("model"=>$model),true);
			Yii::app()->request->sendFile("archivo.xls",$content);
		}
	}
//Vista EXCEL
<table>
	<tr>
		<td>campo1</td>
		<td>campo2</td>
		<td>campo3</td>
	</tr>
	<?php foreach($model as $data): ?>
		<tr>
			<td><?php echo $data->valor1;</td>
			<td><?php echo $data->valor2;</td>
			<td><?php echo $data->valor3;</td>
		</tr>
	<?php endforeach; ?>

</table>	
```
## Component user 🙎‍♂️
_Se encarga de gestionar  la autenticación de los usuarios y paere de los permisos_
```
<?php
//Extendemos la Clase Controller
	class TablaController extends Controller{
		public function actionIndex(){
			if(!Yii::app()->user->isGuest){
				//Si el usuario esta logeado
					Yii::app()->user->setFlash("success","Mensaje");
				//Si queremos guardar la variable de sesión
					Yii::app()->user->setState("MyVarSession","variable");
					Yii::app()->user->getState("MyVarSession");
				//Verificamos si hay una variable de sesiòn
					Yii::app()->user->hasState("MyVarSession");
				//Para logear a un usuario y el tiempo que durara la sesión
					Yi::app()->user->login(CUserIdentity,360*4);
				//Para cerrar sesión del usuario
					Yii::app()->user->logout();
			}
		}
	}
```
## Funcionamiento de un login 👨‍💻
```
<?php
 public function actionLogin(){
  	//Instancianos al formulario
        $model = new LoginForm();
        // Si es una solicitud de validacion ajax
	        if (isset($_POST['ajax']) && $_POST['ajax']==='login-form') {
	            echo CativeForm::validate($model);
	            Yii::app()->end();
	        }
	    //Si la validación es correcta pasa al formulario
	        if(isset($_POST['LoginForm'])){
	        	//Hacen set los campos del formuulario
	        		$model->attributes=$_POST['LoginForm'];
	        	//Valida los datos y redirigir a la página anterior si es valida	
	        		if($model->validate() && $model->login()){
	        			$this->redirect(Yii::app()->user->returnUrl);
	        		} 
	    	}
	    $this->render('login',array('model'=>$model));
}
```
## Modelo del formulario 📃
```
public function login(){
	if($this->_identity==null){
		//Creamos una instancia UserIdentity le pasamos los atributos del modelo dell formulariow
		$this->_identity=new UserIdentity($this->password);
		//Si le des permisos o no al usuario
		$this->_identity->authenticate();
	}
	//Si no hubo errores 
	if($this->_identity->errorCode===UserIdentity::ERROR_NONE){
		//Se ejecuta estas acciones
		//remeberMe si checkeo la casilla de mantenerse logeado 
		$duration=$this->remeberMe ? 3600*24*30 : 0;
		Yii::app()->user->login($this->_indetity,$duration);
		return true;
	}
	else
		return false;
}
```
* [Iconos](https://es.piliapp.com/twitter-symbols/) 🔘

