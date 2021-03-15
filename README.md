# Como ejecutar Driver-app desde Talana con DeepLinking

## Deeplinking

  Es un método que consiste en utilizar un enlace profundo o hipervínculo que dirige directamente a un lugar en específico sin tener que pasar por la entrada principal de la aplicación.


## Dicho esto, veremos un ejemplo de como ejecutar otra app y enviarle información ocupando deeplinking con "Expo" de React-native:

  1. Lo primero es importar 'expo-linking' en la App de la siguiente forma:
  
    import * as Linking from 'expo-linking';
    
  2. Luego creamos una función que en este caso contendrá una funnción asincrona, la cuál se encargará  de recibir los parámetros del path que se creará, y los parametros que se vayan a mandar a la otra app:
  
  ~~~
  
  const linkToB = async (path, param1, param2) => {

  // Aquí se crea la URL
  const url = `wptbobpr://${path}?param1=${param1}&param2=${param2}`

  // Se crea una variable para ver si es posible abrir la URL
  let isPossibleOpenURL = false;
  
  try {
    // Se usa el canOpenURL() de expo-linking, lo que nos retorna un booleano que nos indica si 
    // efectivamente se puede abrir la url
    const isPossibleOpenURL = await Linking.canOpenURL(url);
    
    // Si es posible entonces isPossibleOpenURL será 'TRUE' y se abrirá la otra app con el método
    // openURL de expo-linking
    if (isPossibleOpenURL) {
      await Linking.openURL(url);
    }
  } catch(e) {

    // Si isPossibleOpenURL es 'FALSE' entonces se ejecutará el método openSetting
    // Abrirá la configuración
    if (isPossibleOpenURL) {
      try {
        await Linking.openSettings()
      } catch(e) {
        console.error(e)
      }
    }
  }
  console.log('Alice link to Bob Finish');
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
      <Text>Mock de Alice</Text>
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
      <Button title="Open second App" onPress={ () => linkToB(value, value2, value3) } />
    </View>
  );
}
  
  ~~~


  4. Con esto, obtenemos como resultado la siguiente vista:
  
  ![Ejemplo 1:][./ejemplo1]
  
  En donde a través de los input, podémos setear el path (el cual debe concordar con el path de la aplicación que recibe los datos) y los valores a enviar, y al apretar el botón "Open second App" Automáticamente abrirá la otra aplicación y enviará los parámetros.
