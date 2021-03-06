# Como ejecutar Driver-app desde Talana con DeepLinking

## DeepLinking

  Es un método que consiste en utilizar un enlace profundo o hipervínculo que dirige directamente a un lugar en específico sin tener que pasar por la entrada principal de la aplicación.
  
  Para levantar la aplicación de conductor desde Talana es necesario abrir por DeepLinking la siguiente URL a modo de ejemplo: 
  
      wptdriverapp://login?username=juanito&token=1234567890


## Dicho esto, veremos un ejemplo de como ejecutar otra app y enviarle información ocupando deeplinking con "Expo" de React-native:

  1. Lo primero es importar 'expo-linking' en la App de la siguiente forma:
  
    import * as Linking from 'expo-linking';
    
  2. Luego creamos una función que en este caso contendrá una función asincrona, la cuál se encargará  de recibir los parámetros del path que se creará, y los parametros que se vayan a mandar a la otra app:

  En esta función se ocupan los siguientes métodos de 'expo-linking':

  - canOpenURL(): Se usa para comprobar si efectivamente se están dando las condiciones para poder abrir la otra aplicación,
  la cual debe contener un "eschema" determinado en el App.json de la otra aplicación.
  Nos retorna un booleano que nos indica si efectivamente se puede abrir la url con un 'TRUE' o si no se pudo con un 'FALSE'.

  - openURL(): Este es el método que se usa para poder abrir la otra aplicación una vez que se confirma con el método anterior que está todo correctamente y se puede ejecutar

  
  ~~~
  
  const linkToB = async (scheme, path, username, token) => {
  const url = `${scheme}:/${path}?username=${username}&token=${token}`

  console.log('Path: ' + url);
  let isPossibleOpenURL = false;
    try {
      const isPossibleOpenURL = await Linking.canOpenURL(url);
      if (isPossibleOpenURL) {
        await Linking.openURL(url);
      } else {
        Alert.alert('Advertencia', 'No hay ninguna app registrada con el scheme ' + scheme);
      }
    } catch(e) {
      if (isPossibleOpenURL) {
        try {
          await Linking.openSettings()
        } catch(e) {
          console.error(e)
        }
      }
    }
  }

  
  ~~~
  
  3. Por último, en la vista le enviamos a través de cuadros de texto los valores que deseamos como path o parametros, y el botón ejecuta la función anterior:
  
  ~~~
  
  export default function App() {

  const [value, onChangeText] = useState('path2');
  const [value2, onChangeText2] = useState('Hola');
  const [value3, onChangeText3] = useState('Chao');

  return (
    <View style={styles.container}>
      <Text>Mock de A</Text>
      <StatusBar style="auto" />
      <TextInput
      style={{ height: 40, borderColor: 'gray', borderWidth: 1 }}
      onChangeText={text => onChangeText(text)}
      value={value}
    />
    <TextInput
      style={{ height: 40, borderColor: 'gray', borderWidth: 1 }}
      onChangeText={text => onChangeText2(text)}
      value={value2}
    />
    <TextInput
      style={{ height: 40, borderColor: 'gray', borderWidth: 1 }}
      onChangeText={text => onChangeText3(text)}
      value={value3}
    />
      {/* <Text>Url creada: </Text> */}
      <Button title="Open B" onPress={ () => linkToB(value, value2, value3) } />
    </View>
  );
}
  
  ~~~


  4. Con esto, obtenemos como resultado la siguiente vista:
  
  ![Ejemplo 1:](https://github.com/JorgeArancibia4869/Instructivo/blob/main/ejemplo1.png)

  Para levantar la aplicación DriverApp, en vez de poner "path2" poner "/login", y como argumentos, el usuario indicado y el token.
  
  En donde a través de los input, podémos setear el path (el cual debe concordar con el path de la aplicación que recibe los datos) y los valores a enviar, y al apretar el botón "Open B" Automáticamente abrirá la otra aplicación y enviará los parámetros.
