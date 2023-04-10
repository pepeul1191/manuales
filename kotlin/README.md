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

