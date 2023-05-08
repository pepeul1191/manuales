<b>ui.app.uis.login.viewmodels.LoginViewModel</b>

```kotlin
fun validar(context: Context){
    val id: Int = UserService.validate(usuario.value!!, contrasenia.value!!)
    if(id == 0){
        updateMensaje("Error: Usuario y contraseña no válidos")
    }else{
        updateMensaje("Todo OK")
        Handler().postDelayed({
            updateMensaje("")
            val appActivity =  Intent(context, AppActivity::class.java)
            appActivity.putExtra("user_id", id)
            context.startActivity(
                appActivity
            )
        }, 3000)
    }
}
```

<b>ui.app.uis.app.uis.PokemonScreen</b>

```kotlin
@OptIn(ExperimentalMaterialApi::class)
@Composable
public fun PokemonScreen(
    viewModel: PokemonViewModel,
    navController: NavHostController
){
    val context = LocalContext.current
    viewModel.setPokemons()
    val activity = context as Activity
    val intent = activity?.intent
    var userId = intent.getIntExtra("user_id", 0)
    var isBottomSheetOpen by rememberSaveable { mutableStateOf(false) }
  .
  .
  .
   Column(
    ) {
        TopBar(
            showBottomSheetDialog = {
                isBottomSheetOpen = true
            },
            navController,
            userId
        )
```

<b>ui.navigations.uis.TopBar</b>

```kotlin
@Preview
@Composable
fun TopBarPreview(){
    TopBar(
        showBottomSheetDialog = {},
        rememberNavController(),
        0
    )
}

@Composable
fun TopBar(
    showBottomSheetDialog: () -> Unit,
    navController: NavHostController,
    userId: Int
){
    Box(
        modifier = Modifier
            .height(80.dp)
            .fillMaxWidth()
    ){
        TopAppBar(
            //modifier = Modifier.padding(bottom =  24.dp),
            //backgroundColor = Color(R.color.purple_500),
            elevation = 0.dp,
            title = {
                Text(
                    text = "Aplicación Demo",
                    color = Color.White
                )
            },
            actions = {
                AppBarActions(showBottomSheetDialog, navController, userId)
            }
        )
    }
}

@Composable
fun AppBarActions(
    showBottomSheetDialog: () -> Unit,
    navController: NavHostController,
    userId: Int
){
    SearchAction()
    ShareAction(showBottomSheetDialog)
    MoreAction(navController, userId)
}

@Composable
fun SearchAction(){
    val context = LocalContext.current
    IconButton(
        onClick = {
            Toast.makeText(context,"Buscar???", Toast.LENGTH_SHORT).show()
        }
    ){
        Icon(
            imageVector = Icons.Filled.Search,
            contentDescription = "icono buscar",
            tint = Color.White
        )
    }
}

@Composable
fun ShareAction(showBottomSheetDialog: () -> Unit){
    val launcher = rememberLauncherForActivityResult(
        contract = ActivityResultContracts.StartActivityForResult(),
        onResult = { /* Handle activity result */ }
    )

    val context = LocalContext.current
    var launched = false
    IconButton(
        onClick = {
            showBottomSheetDialog()
            val intent = Intent(Intent.ACTION_SEND)
            intent.type = "text/plain"
            intent.putExtra(Intent.EXTRA_TEXT, "https://www.ulima.edu.pe/")
            launcher.launch(intent)
        }
    ){
        Icon(
            imageVector = Icons.Filled.Share,
            contentDescription = "icono compartir",
            tint = Color.White
        )
    }
}

@Composable
fun MoreAction(
    navController: NavHostController,
    userId: Int
){
    var expanded by remember { mutableStateOf(false)}
    val context = LocalContext.current
    IconButton(
        onClick = {
            expanded = true
        }
    ){
        Icon(
            imageVector = Icons.Filled.MoreVert,
            contentDescription = "icono más",
            tint = Color.White
        )
        DropdownMenu(
            expanded = expanded,
            onDismissRequest = {
                expanded = false
            }
        ) {
            DropdownMenuItem(
                onClick = {
                    expanded = false
                    //context.startActivity(Intent(context, UploadActivity::class.java))
                    navController.navigate("/profile/?user_id=$userId")
                }
            ){
                Text(
                    text = "Editar Perfil"
                )
            }
            DropdownMenuItem(
                onClick = {
                    expanded = false
                    (context as? Activity)?.run {
                        finishAffinity()
                    }
                }
            ){
                Text(
                    text = "Salir"
                )
            }
        }
    }
}
```

<b>ui.app.uis.PokemonDetailScreen</b>

```kotlin
@Composable
public fun PokemonDetailScreen(
    viewModel: PokemonDetailViewModel
){
    // viewmodel
    val nombre: String by viewModel.name.observeAsState("")
    val url: String by viewModel.url.observeAsState("")
    val peso: Float by viewModel.peso.observeAsState(0f)
    val talla: Float by viewModel.talla.observeAsState(0f)
    val titulo: String by viewModel.titulo.observeAsState("")

    Column() {
        Row(
            modifier = Modifier.fillMaxWidth(),
            verticalAlignment = Alignment.CenterVertically
        ){
            Text(
                text =titulo.toUpperCase(),
                textAlign = TextAlign.Center,
                modifier = Modifier.padding(16.dp),
                fontSize = 24.sp
            )
        }
        Row(
            modifier = Modifier.fillMaxWidth()
        ){
            Image(
                painter = rememberImagePainter(data = url),
                contentDescription = nombre,
                modifier = Modifier
                    .size(80.dp)
                    .padding(bottom = 10.dp),
            )
            Column(
                modifier = Modifier
                    .weight(1f)
                    .align(Alignment.CenterVertically)
            ){
                TextField(
                    value = nombre,
                    onValueChange = {
                        viewModel.updateName(it)
                    },
                    modifier = Modifier.fillMaxWidth(),
                    label = {
                        Text(text = "Nombre")
                    },
                    placeholder = {
                        Text(text= "")
                    },
                    singleLine = true,
                    colors = TextFieldDefaults.textFieldColors(
                        backgroundColor = Color.Transparent
                    )
                )
                TextField(
                    value = "${peso.toString()} kg" ,
                    onValueChange = {
                        viewModel.updatePeso(it.toFloat())
                    },
                    modifier = Modifier.fillMaxWidth(),
                    label = {
                        Text(text = "Peso Kg")
                    },
                    placeholder = {
                        Text(text= "")
                    },
                    singleLine = true,
                    colors = TextFieldDefaults.textFieldColors(
                        backgroundColor = Color.Transparent
                    )
                )
                TextField(
                    value = "${talla.toString()} m" ,
                    onValueChange = {
                        viewModel.updateTalla(it.toFloat())
                    },
                    modifier = Modifier.fillMaxWidth(),
                    label = {
                        Text(text = "Talla en metros")
                    },
                    placeholder = {
                        Text(text= "")
                    },
                    singleLine = true,
                    colors = TextFieldDefaults.textFieldColors(
                        backgroundColor = Color.Transparent
                    )
                )
                Button(
                    modifier = Modifier
                        .fillMaxWidth()
                        .padding(top = 15.dp/*, start = 40.dp, end = 40.dp*/), // start -> izquierda, end -> derecha
                    onClick = {

                    }
                ){
                    Text(
                        "${ titulo.toUpperCase().split(" ")[0] } REGISTRO"
                    )
                }
                // shares buttons
                val context = LocalContext.current
                val launcher = rememberLauncherForActivityResult(
                    contract = ActivityResultContracts.StartActivityForResult(),
                    onResult = {
                        println("COMPARTIDOOOOOOOOOOOOO")
                    }
                )
                val coroutineScope = rememberCoroutineScope()
                // facebook
                Button(
                    modifier = Modifier
                        .fillMaxWidth()
                        .padding(top = 1.dp, /*start = 40.dp, end = 40.dp*/),
                    onClick = {
                        val intent = Intent(Intent.ACTION_SEND)
                        intent.type = "text/plain"
                        val appPackageName = "com.facebook.katana"
                        intent.putExtra(Intent.EXTRA_TITLE, "Has selecinado un $nombre".toUpperCase())
                        intent.putExtra(Intent.EXTRA_TEXT, url)
                        intent.setPackage(appPackageName)
                        if (intent.resolveActivity(context.packageManager) != null) {
                            launcher.launch(intent)
                        }
                    },
                    //colors = ButtonDefaults.buttonColors(backgroundColor = Color(0xFF4CAF50)) ,
                    colors = ButtonDefaults.buttonColors(backgroundColor = Color.Blue)
                ){
                    Image(
                        painter = painterResource(id = RLocal.drawable.ic_facebook),
                        contentDescription = "Logo Facebook",
                        modifier = Modifier
                            .size(22.dp)
                            .padding(end = 10.dp),
                        colorFilter = ColorFilter.tint(Color.White)
                    )
                    Text(
                        "Compartir en Facebook",
                        color = Color.White
                    )
                }
                // whastapp
                Button(
                    modifier = Modifier
                        .fillMaxWidth()
                        .padding(top = 1.dp, /*start = 40.dp, end = 40.dp*/),
                    onClick = {
                        coroutineScope.launch {
                            val imageLoader = ImageLoader(context)
                            val request = ImageRequest.Builder(context)
                                .data(url)
                                .build()
                            val bitmap = (imageLoader.execute(request) as SuccessResult).drawable.toBitmap()
                            val uri = context.contentResolver.insert(MediaStore.Images.Media.EXTERNAL_CONTENT_URI, ContentValues())?.apply {
                                context.contentResolver.openOutputStream(this).use {
                                    bitmap.compress(Bitmap.CompressFormat.JPEG, 100, it)
                                }
                            }
                            val intent = Intent(Intent.ACTION_SEND)
                            intent.type = "image/jpeg"
                            val appPackageName = "com.whatsapp"
                            intent.putExtra(Intent.EXTRA_TITLE, "Has selecinado un $nombre".toUpperCase())
                            intent.putExtra(Intent.EXTRA_STREAM, uri)
                            intent.putExtra(Intent.EXTRA_TEXT, "Has selecinado un ${nombre.toUpperCase()}")
                            intent.setPackage(appPackageName)
                            if (intent.resolveActivity(context.packageManager) != null) {
                                launcher.launch(intent)
                            }
                        }
                    },
                    //colors = ButtonDefaults.buttonColors(backgroundColor = Color(0xFF4CAF50)) ,
                    colors = ButtonDefaults.buttonColors(backgroundColor = Color.Green)
                ){
                    Image(
                        painter = painterResource(id = RLocal.drawable.ic_whatsapp),
                        contentDescription = "Logo Whatsapp",
                        modifier = Modifier
                            .size(22.dp)
                            .padding(end = 10.dp),
                        colorFilter = ColorFilter.tint(Color.White)
                    )
                    Text(
                        "Compartir en WhastApp",
                        color = Color.White
                    )
                }
            }
        }
    }
}