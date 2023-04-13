```  kotlin

open class Animal {
    open fun fastidiar(){
        println("Este animalito fastidia")
    }
}

data class Gato(
    var raza: String = "",
    var nombre: String = "",
    var peso: Float = 0F,
    var talla: Float = 0F
): Animal(){
    fun trepar(){
        println("este gato trepa")
    }
}

class Perro: Animal(
) {
    var raza: String = ""
    var peso: Float = 0.1F
    var talla: Float = 2.123F
    var nombre: String = "Firulais"

    init {
        println("Estamos creando una instancia de perro")
    }

    override fun fastidiar(){
        println("el perro fastidia")
    }
}

class Libreria {
    companion object {
        fun aletarioEntero(): Int{
            return Random.nextInt(1, 101)
        }
    }
}


package pe.edu.ulima.kotlinlearn

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import pe.edu.ulima.kotlinlearn.daos.UserDAO
import pe.edu.ulima.kotlinlearn.models.Gato
import pe.edu.ulima.kotlinlearn.models.Libreria
import pe.edu.ulima.kotlinlearn.models.Perro
import pe.edu.ulima.kotlinlearn.models.User
import kotlin.random.Random

class MainActivity : AppCompatActivity() {
    fun demo(): Int? {
        val randomNumber = Random.nextInt(1, 101)
        if (randomNumber % 2 == 0){
            return randomNumber
        }else{
            return null
        }
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        println("+++++++++++++++++++++++++++++++++++++++++++++")
        println("run")
        var x: Int = 2
        var y: String = "hola"
        println(2+4)
        println("antes " + x)
        x = 2 + 4
        println("despues " + x.plus(4))
        println("despues $x, otra notación")
        println("y = $y y teine ${y.length} letras")
        val demoRpta: Int? = demo()
        println(demoRpta)
        if(demoRpta != null){
            var x1 = demoRpta!! + 2
            println(x1)
        }
        var x1 = (demoRpta?: 0) + 2
        println(x1)
        println("+++++++++++++++++++++++++++++++++++++++++++++")
        var p1 = Perro()
        p1.nombre = "Sila"
        p1.peso = 2.3F
        p1.talla = 4.3F
        println(p1.toString())
        p1.fastidiar()
        var g1 = Gato()
        g1.nombre = "Copito"
        println(g1.toString())
        g1.fastidiar()
        g1.trepar()
        println(Libreria.aletarioEntero())
        setContentView(R.layout.activity_main)
        println("+++++++++++++++++++++++++++++++++++++++++++++")
        var tmpGatosList = listOf(
            Gato("Persa", "Copito", 23F, 3F),
            Gato("Chino", "Miau", 23F, 3F),
            Gato("Peruano", "Persian", 23F, 3F)
        )
        var gatos: ArrayList<Gato> = ArrayList(tmpGatosList)
        println(gatos.size)
        gatos.add(
            Gato("Persa", "Nabuconodosor", 23F, 3F)
        )
        println(gatos.size)
        val tabla = mutableMapOf<String, Int>()
        tabla.put("estado_civil",2)
        tabla.put("peso",70)
        println(tabla)
        for (fruit in gatos) {
            print("$fruit ")
        }
    }
}
```
```  kotlin

    fun esPrimo(numero: Int): Boolean{
        var numeros = 0
        println("+++++++++++++++++++++ $numero")
        for (i in 1.. numero/2){
            if (numero % i == 0){
                println(i)
                numeros++
            }
            if(numeros == 2){
                return false
            }
        }
        return true
    }

    fun contarLetras(palabra: String): Map<Char, Int>{
        var diccionario = mutableMapOf<Char, Int>()
        for(i in 0 .. (palabra.length - 1)){
            val letra: Char = palabra[i]
            if (diccionario.containsKey(letra)){
                val tmp: Int = diccionario[letra]!!
                diccionario[letra] = tmp + 1
            }else{
                diccionario[letra] = 1
            }
        }
        return diccionario
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        println("1 ++++++++++++++++++++++++++++++++++++++++++++")
        val tmpList = listOf(
            Perro("Cupressus funebris Endl.", "Issy-les-Moulineaux", 10.9894027572f, 75.5727460732f),
            Perro("Colubrina texensis (Torr. & A. Gray) A. Gray var. texensis", "Meru-Kinna", 27.7359787232f, 85.997455665f),
            Perro("Graphina subnitida (Nyl.) Zahlbr.", "Port Said", 21.3007754421f, 36.3371738415f),
            Perro("Maronea carolinae H. Magn.", "", 7.4555111268f, 52.5756427979f),
            Perro("Ptelea trifoliata L. ssp. angustifolia (Benth.) V. Bailey", "Pamplona", 24.2318514295f, 60.863559451f),
            Perro("Polygonatum biflorum (Walter) Elliott var. necopinum R. Ownbey", "Aberdeen", 57.935954847f, 86.6843435541f),
            Perro("Phacelia crenulata Torr. ex S. Watson var. minutiflora (J. Voss) Jeps.", "Hill City", 45.2516545347f, 87.1630015044f),
            Perro("Lupinus nanus Douglas ex Benth. ssp. nanus", "Minsk", 58.4744083875f, 99.8805397573f),
            Perro("Gratiola pilosa Michx.", "Davenport", 35.2042520699f, 66.9487868926f),
            Perro("Acacia doratoxylon A. Cunn.", "", 19.2443513829f, 65.8394425016f),
            Perro("Maronea polyphaea H. Magn.", "Pirassununga", 13.064842678f, 94.1504197517f),
            Perro("Elymus glaucus Buckley ssp. glaucus", "Itaperuna", 13.3212725967f, 69.901427492f),
            Perro("Arnica amplexicaulis Nutt.", "Puerto Leguízamo", 7.7957522778f, 80.7373150334f),
            Perro("Acarospora strigata (Nyl.) Jatta", "Lac Brochet", 22.342227352f, 60.2194865651f),
            Perro("Crataegus ×media Bechst.", "Langebaanweg", 50.8018645271f, 51.5512053726f),
            Perro("Wolffia Horkel ex Schleid.", "Chihuahua", 50.6263146181f, 74.2106611734f),
            Perro("Croton heterocarpus Müll. Arg.", "Växjö", 59.4967238921f, 62.7176212428f),
            Perro("Lonicera caerulea L. var. cauriana (Fernald) B. Boivin", "Thunder Bay", 23.1256899938f, 42.3757668144f),
            Perro("Verbena scabra Vahl", "Riverside", 9.3842852096f, 90.501853482f),
            Perro("Salix lapponum L.", "Subang", 14.3272747294f, 67.4738666518f),
            Perro("Psidium guajava L.", "High Prairie", 36.1301846169f, 83.7404844893f),
            Perro("Spermacoce L.", "Charlotte", 8.7516130943f, 65.0694869629f),
            Perro("Symphytum ×uplandicum Nyman (pro sp.)", "Propriá", 29.5324123019f, 38.4250486063f),
            Perro("Cardaria pubescens (C.A. Mey.) Jarmolenko", "Livermore", 7.3872452642f, 86.1892551073f),
            Perro("Scleria pauciflora Muhl. ex Willd. var. caroliniana (Willd.) Alph. Wood", "Pardubice", 54.2125398701f, 96.9579193974f),
            Perro("Plantago rugelii Decne.", "Santiago de Méndez", 32.3753074902f, 96.0680322688f),
            Perro("Flaveria floridana J.R. Johnst.", "Kirakira", 26.6825851861f, 40.7186642538f),
            Perro("Linanthus demissus (A. Gray) Greene", "David", 59.2999194504f, 59.0157274056f),
            Perro("Cyperus difformis L.", "Riga", 57.2538574964f, 99.5044438969f),
            Perro("Geum ×catlingii J.-P. Bernard & R. Gauthier", "Lábrea", 31.623409273f, 67.5697133164f),
            Perro("Lilium grayi S. Watson", "San Luis", 57.9147601096f, 61.5492211842f),
            Perro("Pertusaria propinqua Müll. Arg.", "", 17.0143434417f, 40.4840475068f),
            Perro("Melica porteri Scribn. var. laxa Boyle", "Berlin", 28.7025372992f, 34.0555887329f),
            Perro("Cardamine pachystigma (S. Watson) Rollins var. dissectifolia (Detling) Rollins", "Malekula Island", 17.9507061158f, 36.154755853f),
            Perro("Graphina virginea (Eschw.) Müll. Arg.", "Bursa", 41.3006798316f, 39.4661845688f),
            Perro("Ribes marshallii Greene", "Cuxhaven", 57.1059709723f, 70.541854118f),
            Perro("Matelea balbisii (Decne.) Woodson", "Kings Canyon Resort", 53.0916468484f, 50.5480701138f),
            Perro("Digitalis purpurea L. ssp. purpurea", "Vunisea", 8.147007838f, 64.1423555547f),
            Perro("Arctostaphylos insularis Greene ex Parry", "Lethem", 42.2874716831f, 39.0579469866f),
            Perro("Arctoa fulvella (Dicks.) Bruch & Schimp.", "Borrego Springs", 13.3911041311f, 88.8898015057f)
        )
        var perros: ArrayList<Perro> = ArrayList(tmpList)
        var menorPeso: Float = 90999999f
        var mayorPeso: Float = 0f
        var pesoAcumulado: Float = 0f
        for(perro: Perro in perros){
            println(perro.nombre)
            if(perro.peso > mayorPeso){
                mayorPeso = perro.peso
            }
            if(perro.peso < menorPeso){
                menorPeso = perro.peso
            }
            pesoAcumulado += perro.peso
        }
        println("peso mayor: ${mayorPeso} kg")
        println("peso mayor: ${menorPeso} kg")
        println("peso acumulado: ${pesoAcumulado} kg")
        println(esPrimo(4))
        println(esPrimo(14))
        println(esPrimo(17))
        println(esPrimo(15))
        println(esPrimo(29))
        println(esPrimo(3))
        val str = "pepito"
        println("Contrar letras de $str")
        println(contarLetras(str))
        println("2 ++++++++++++++++++++++++++++++++++++++++++++")

        setContentView(R.layout.activity_semana02)
```  