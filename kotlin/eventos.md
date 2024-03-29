### Swipe Back

```kotlin
import androidx.compose.ui.platform.LocalContext
import android.app.Activity

@Composable
public fun XYZScreen(){
  val context = LocalContext.current as Activity
  ...
  BackHandler {
    context.finish()
  }
  ...
}
```

### Close Button

```kotlin
import androidx.compose.ui.platform.LocalContext
import android.app.Activity

@Composable
public fun XYZScreen(){
  val context = LocalContext.current as Activity
  ...
  Box(modifier = Modifier.fillMaxSize()) {
    IconButton(
      onClick = {
        context.finish()
      },
      modifier = Modifier.align(Alignment.TopEnd).padding(10.dp)
    ){
      Icon(
        imageVector = Icons.Default.Close,
        contentDescription = "Close Icon",
      )
    }
  }
  ...
}
```


```kotlin
package pe.edu.ulima.ui.app.viewmodels

import android.net.Uri
import androidx.compose.runtime.mutableStateOf
import androidx.compose.ui.res.painterResource
import androidx.lifecycle.LiveData
import androidx.lifecycle.MutableLiveData
import androidx.lifecycle.ViewModel
import kotlinx.coroutines.flow.MutableStateFlow
import kotlinx.coroutines.flow.StateFlow
import pe.edu.ulima.R
import java.io.File

class PokemonDetailViewModel: ViewModel() {
    private val _uri = mutableStateOf<Uri?>(
     null
    )
    val uri get() = _uri.value
    fun setUri(uri: Uri) {
        _uri.value = uri
    }

    private val _name = MutableLiveData<String>("")
    var name: LiveData<String> = _name
    fun updateName(it: String){
        _name.postValue(it)
    }

    private val _uri2 = MutableLiveData<Uri>(null)
    var uri2: LiveData<Uri> = _uri2
    fun updateUri2(it: Uri){
        _uri2.postValue(it)
    }

    private val _generationName = MutableLiveData<String>("")
    var generationName: LiveData<String> = _generationName
    fun updateGenerationName(it: String){
        _generationName.postValue(it)
    }
}
```

```kotlin
package pe.edu.ulima.ui.app.uis

import android.annotation.SuppressLint
import android.app.Activity
import android.content.Intent
import android.graphics.Bitmap
import android.graphics.ImageDecoder
import android.net.Uri
import android.os.Build
import android.provider.MediaStore
import android.util.Log
import pe.edu.ulima.R
import androidx.activity.compose.rememberLauncherForActivityResult
import androidx.activity.result.contract.ActivityResultContracts
import androidx.annotation.RequiresApi
import androidx.compose.foundation.Image
import androidx.compose.foundation.clickable
import androidx.compose.foundation.layout.*
import androidx.compose.runtime.*
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.painter.Painter
import androidx.compose.ui.platform.LocalContext
import androidx.compose.foundation.lazy.items
import androidx.compose.material.*
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.KeyboardArrowDown
import androidx.compose.material.icons.filled.KeyboardArrowUp
import androidx.compose.runtime.livedata.observeAsState
import androidx.compose.ui.geometry.Size
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.graphics.Color.Companion.Green
import androidx.compose.ui.graphics.Color.Companion.Transparent
import androidx.compose.ui.graphics.Color.Companion.White
import androidx.compose.ui.graphics.Color.Companion.Yellow
import androidx.compose.ui.graphics.asImageBitmap
import androidx.compose.ui.layout.onGloballyPositioned
import androidx.compose.ui.platform.LocalDensity
import androidx.compose.ui.res.painterResource
import androidx.compose.ui.res.stringResource
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.toSize
import coil.compose.rememberImagePainter
import pe.edu.ulima.ui.app.viewmodels.PokemonDetailViewModel

@Preview
@Composable
@RequiresApi(Build.VERSION_CODES.P)
public fun PokemonDetailScreenPreview(){
    PokemonDetailScreen(
        PokemonDetailViewModel()
    )
}

@SuppressLint("UnrememberedMutableState")
@Composable
@RequiresApi(Build.VERSION_CODES.P)
public fun PokemonDetailScreen(
    viewModel: PokemonDetailViewModel,
){
    val context = LocalContext.current as Activity
    var imageUri by remember { mutableStateOf<Uri?>(null) }
    var uri2 by remember { mutableStateOf(viewModel.uri) }
    val bitmap = remember { mutableStateOf<Bitmap?>(null)}
    var expanded by remember { mutableStateOf(false) }
    val list = listOf("Kotlin", "Java", "Dart", "Python")
    var selectedItem by remember { mutableStateOf("") }
    var textFiledSize by remember { mutableStateOf(Size.Zero) }
    val icon = if (expanded){
        Icons.Filled.KeyboardArrowUp
    }else{
        Icons.Filled.KeyboardArrowDown
    }
    // viewmodel map
    val name: String by viewModel.name.observeAsState(initial = "")
    val uri3: Uri? by viewModel.uri2.observeAsState(initial = null)
    val generationName: String by viewModel.generationName.observeAsState(initial = "")

    val launcher = rememberLauncherForActivityResult(
        // ActivityResultContracts.StartActivityForResult(),
        contract = ActivityResultContracts.GetContent()
    ) {
            uri: Uri? ->
                if (uri != null){
                    viewModel.updateUri2(uri)
            }
    }

    Column(
        modifier = Modifier.padding(start = 20.dp, end = 20.dp)
    ){
        uri3?.let {
            if(Build.VERSION.SDK_INT < 24){
                bitmap.value = MediaStore.Images.Media.getBitmap(context.contentResolver, it)
            }else{
                val source = ImageDecoder.createSource(context.contentResolver, it)
                bitmap.value = ImageDecoder.decodeBitmap(source)
            }
            viewModel.setUri(uri3!!)
        }
        if (bitmap.value == null){
            Image(
                painter = painterResource(id = R.drawable.ic_default_image),
                contentDescription = null,
                modifier = Modifier
                    .size(400.dp)
                    .padding(20.dp)
            )
        }else{
            Image(
                bitmap = bitmap.value!!.asImageBitmap(),
                contentDescription = null,
                modifier = Modifier
                    .size(400.dp)
                    .padding(20.dp)
            )
        }
        Button(
            modifier = Modifier.fillMaxWidth(),
            onClick = {
                launcher.launch("image/*")
            }
        ) {
            Text(text = "Seleccionar Imagen")
        }
        // name
        TextField(
            value = name,
            onValueChange = {
                viewModel.updateName(it)
            },
            modifier = Modifier.fillMaxWidth(),
            label = {
                Text(text = "Nombre del Pokemon")
            },
            placeholder = {
                Text(text= "")
            },
            singleLine = true,
            colors = TextFieldDefaults.textFieldColors(
                backgroundColor = Color.Transparent
            )
        )
        // generación
        OutlinedTextField(
            value = generationName,
            enabled = false,
            onValueChange = {
                viewModel.updateGenerationName(it) },
            modifier = Modifier
                .fillMaxWidth()
                .onGloballyPositioned { coordinates ->
                    textFiledSize = coordinates.size.toSize()
                },
            label = {Text(text="Generación de Pokemon")},
            colors = TextFieldDefaults.outlinedTextFieldColors(
                focusedBorderColor = Green,
                unfocusedBorderColor = Transparent,
            disabledTextColor = White),
            trailingIcon = {
                Icon(icon, "", Modifier.clickable{
                    expanded = !expanded
                })
            }
        )
        DropdownMenu(
            expanded = expanded,
            onDismissRequest = {
                expanded = false
            },
            modifier = Modifier
                .width(with(LocalDensity.current){textFiledSize.width.toDp()})
        ) {
            list.forEach { label ->
                DropdownMenuItem(
                    onClick = {
                        viewModel.updateGenerationName(label)
                        expanded = false
                    }                ) {
                    Text(text = label)
                }
            }
        }
        // checkbox
        Row(){
            val checkStateJava = remember {
                mutableStateOf(false)
            }

            Checkbox(
                checked = checkStateJava.value,
                onCheckedChange = {
                    checkStateJava.value = it
                },

            )
            Text(
                text = "Java",
                modifier = Modifier.padding(start = 5.dp, top = 12.dp)
            )
        }
        // hr
        Divider(
            modifier = Modifier
                .fillMaxWidth()
                .padding(top = 10.dp, bottom = 10.dp),
            thickness = 2.dp,
        )
        Button(
            onClick = {
                Log.d("POKEMON_DETAIL_SCREEN", viewModel.uri2.value.toString())
                Log.d("POKEMON_DETAIL_SCREEN", viewModel.name.value.toString())
                Log.d("POKEMON_DETAIL_SCREEN", viewModel.generationName.value.toString())
            }
        ) {
            Text(text = "Ver Modelo")
        }
    }
}
```

```kotlin
package pe.edu.ulima.ui.app.uis

import android.util.Log
import androidx.compose.foundation.background
import androidx.compose.foundation.gestures.forEachGesture
import androidx.compose.foundation.layout.Box
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.fillMaxWidth
import androidx.compose.foundation.layout.size
import androidx.compose.runtime.Composable
import androidx.compose.runtime.mutableStateOf
import androidx.compose.runtime.remember
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.input.pointer.pointerInput
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import java.lang.Math.sqrt
import kotlin.math.pow
import androidx.compose.foundation.gestures.detectVerticalDragGestures


@Preview
@Composable
public fun TouchScreenPreview(){
    TouchScreen()
}

@Composable
public fun TouchScreen(){
    val size = remember{mutableStateOf(200)}

    Box(
      modifier = Modifier.fillMaxSize(),
      contentAlignment = Alignment.Center
    ){
        Box(
            modifier = Modifier
                .size(size.value.dp)
                .background(Color.Gray)
                .pointerInput(Unit){
                    val fingerCount = 2
                    var previousDistance = 0f
                    detectVerticalDragGestures {  _, dragAmount ->
                        val isUp = dragAmount < 0
                        Log.d("TOUCH_SCREEN", "+++++++++++++++++++++++++++++++++++")
                        Log.d("TOUCH_SCREEN", isUp.toString())
                    }
                    forEachGesture{
                        awaitPointerEventScope {
                            do{
                                val event = awaitPointerEvent()
                                if(event.changes.size == fingerCount){
                                    Log.d("TOUCH_SCREEN", "dos dedos")
                                    val firstX = event.changes[0].position.x
                                    val firstY = event.changes[0].position.y
                                    val secondX = event.changes[1].position.x
                                    val secondY = event.changes[1].position.y
                                    event.changes[0]
                                    val currentDistance = sqrt(
                                        ((secondX - firstX).pow(2) + (secondY - firstY).pow(2)).toDouble()
                                    )

                                    if(currentDistance - previousDistance > 10){
                                        Log.d("TOUCH_SCREEN", "bigger")
                                        if(size.value < 300){
                                            size.value += 3
                                        }
                                        previousDistance = currentDistance.toFloat()
                                    }else if(previousDistance - currentDistance > 10){
                                        Log.d("TOUCH_SCREEN", "smaller")
                                        if(size.value > 150){
                                            size.value -= 3
                                        }
                                        previousDistance = currentDistance.toFloat()
                                    }
                                }
                            }while(event.changes.any{
                                it.pressed
                            })
                        }
                    }
                }
        )
    }
}
```