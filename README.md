# Yii
## Crear una clase 
```
class public function saludo {
	return "Saludo";
}
```
## Para importar una clase se crea el archivo con el mismo nombre de la clase
## la incluimos en el controlador
```
Yii::import('application.nomClase');
Yinclude(Yii::getPathOfAlias('application','nomClase.php'));
```
## La utilizamos en el controlador
```
$me = new nomClase;
echo $me -> saludo();
```
