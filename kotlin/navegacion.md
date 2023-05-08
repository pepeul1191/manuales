
```kotlin
// MVVM
implementation "androidx.lifecycle:lifecycle-viewmodel-ktx:2.4.1"
implementation "androidx.lifecycle:lifecycle-livedata-ktx:2.4.1"
implementation "androidx.compose.runtime:runtime-livedata:$compose_ui_version"
// navigation
implementation "androidx.navigation:navigation-compose:2.4.0-alpha10"
```

```kotlin
data class Pokemon(
    var id: Int = 0,
    var nombre: String = "",
    var peso: Float = 0F,
    var talla: Float = 0F,
    var url: String = "",
)

var pokemonsList = listOf(
    Pokemon(1, "BULBASAUR", 12F, 13F, "https://pokefanaticos.com/pokedex/assets/images/pokemon_imagenes/1.png"),
    Pokemon(2, "IVYSAUR", 22F, 23F, "https://pokefanaticos.com/pokedex/assets/images/pokemon_imagenes/2.png"),
    Pokemon(3, "VENUSAUR", 32F, 33F, "https://pokefanaticos.com/pokedex/assets/images/pokemon_imagenes/3.png"),
    Pokemon(4, "PIKACHU", 32F, 33F, "https://pokefanaticos.com/pokedex/assets/images/pokemon_imagenes/33.png"),
)
```

```kotlin
class MainActivity : ComponentActivity() {
    @RequiresApi(Build.VERSION_CODES.P)
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        // screen navigation
        var pokemonScreenModel = PokemonViewModel()
        var pokemonDetailViewModel = PokemonDetailViewModel()
        setContent {
            ProgramMovilTheme {
                // A surface container using the 'background' color from the theme
                Surface(
                    modifier = Modifier.fillMaxSize(),
                    color = MaterialTheme.colors.background
                ) {
                    AppNavigation(
                        pokemonScreenModel = pokemonScreenModel,
                        pokemonDetailViewModel = pokemonDetailViewModel
                    )
                }
            }
        }
    }
}

@RequiresApi(Build.VERSION_CODES.P)
@Composable
fun AppNavigation(
    pokemonScreenModel: PokemonViewModel,
    pokemonDetailViewModel: PokemonDetailViewModel,
){
    val navController = rememberNavController()
    val navBackStackEntry by navController.currentBackStackEntryAsState()
    val pokemonIdParam = navBackStackEntry?.arguments?.getInt("pokemon_id")

    NavHost(
        navController = navController,
        startDestination = "/pokemon"
    ){
        composable(
            route = "/pokemon",
            arguments = listOf()
        ){
            entry ->
                PokemonScreen(
                    pokemonScreenModel,
                    goToPokemonDetail = {
                        navController.navigate("/pokemon?pokemon_id=${pokemonScreenModel.selectedId}")
                    }
                )
        }
        composable(
            route = "/pokemon?pokemon_id={pokemon_id}",
            arguments = listOf(
                navArgument("pokemon_id") {
                    type = NavType.IntType
                    defaultValue = 0
                }
            )
        ){ entry ->
            pokemonDetailViewModel.getPokemon(pokemonIdParam!!)
            PokemonDetailScreen(pokemonDetailViewModel)
        }
    }
}

@Preview
@Composable
public fun PokemonScreenPreview(){
    PokemonScreen(
        PokemonViewModel(),
        goToPokemonDetail = {}
    )
}

@Composable
public fun PokemonScreen(
    viewModel: PokemonViewModel,
    goToPokemonDetail: () -> Unit
){
    viewModel.setPokemons()
    Column(){
        for (pokemon in viewModel.pokemons!!) {
            Row(
                modifier = Modifier.fillMaxWidth()
            ){
                Image(
                    painter = rememberImagePainter(data = pokemon.url),
                    contentDescription = pokemon.nombre,
                    modifier = Modifier
                        .size(80.dp)
                        .padding(bottom = 10.dp),
                )
                Column(
                    modifier = Modifier
                        .weight(1f)
                        .align(Alignment.CenterVertically)
                ){
                    Text("Nombre: ${pokemon.nombre}")
                    Text("Peso: ${pokemon.peso} kg")
                    Text("Tall: ${pokemon.talla} m")
                    Button(
                        modifier = Modifier
                            .fillMaxWidth()
                            .padding(top = 15.dp/*, start = 40.dp, end = 40.dp*/), // start -> izquierda, end -> derecha
                        onClick = {
                            viewModel.setSelectedId(pokemon.id)
                            goToPokemonDetail()
                        }
                    ){
                        Text("VER DETALLE")
                    }
                }
            }
        }
        Row(){
            Button(
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(top = 15.dp/*, start = 40.dp, end = 40.dp*/), // start -> izquierda, end -> derecha
                onClick = {
                    viewModel.setSelectedId(2)
                    goToPokemonDetail()
                }
            ){
                Text("VER DETALLE EN DURO")
            }
        }
    }
}
```