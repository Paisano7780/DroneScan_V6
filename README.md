Documento de Respaldo: Modificaciones en el Proyecto DJI SDK Sample v5
Este documento detalla los cambios realizados en los archivos principales del proyecto DJI SDK Sample v5 para integrar un escáner de códigos de barras, incluyendo el código final y las soluciones a los errores encontrados.

1. Archivo Modificado: AndroidManifest.xml
Objetivo: Declarar la nueva actividad (ScanActivity) y configurar sus permisos y propiedades.

Ubicación: sample > src > main > AndroidManifest.xml

Cambios Realizados (Código Final):

Dentro de la etiqueta <application>, añade la siguiente declaración de <activity>:

XML

        <activity
            android:name="com.dji.sampleV5.barcodescanner.ScanActivity"
            android:exported="false"
            android:screenOrientation="portrait"
            android:label="Lector de Barcodes">
            </activity>
Solución de Errores / Consideraciones:

Error: Si com.dji.sampleV5.barcodescanner.ScanActivity aparecía en rojo, era debido a que la actividad no estaba declarada en el Manifest. Añadir este bloque resolvió el problema.

android:exported="false": Se utiliza para evitar que la actividad sea lanzada por otras aplicaciones, ya que será lanzada internamente desde DJIAircraftMainActivity.

android:screenOrientation="portrait": Fuerza la orientación vertical para la ScanActivity.

android:label="Lector de Barcodes": Define el título de la actividad.

2. Archivo Nuevo: ScanActivity.kt
Objetivo: Crear la actividad que manejará la lógica del escáner de códigos de barras.

Ubicación: sample > src > main > java > com.dji.sampleV5.barcodescanner > ScanActivity.kt (Crear este paquete y archivo)

Código Final:

Kotlin

package com.dji.sampleV5.barcodescanner

import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity

class ScanActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        // Por ahora, no hay UI definida (setContentView), solo la creamos
        // La definiremos en un paso posterior (activity_scan.xml)
        // setContentView(R.layout.activity_scan) // Esto se añadirá después
    }
}
Solución de Errores / Consideraciones:

Error inicial: El paquete com.dji.sampleV5.barcodescanner podía aparecer en rojo. La solución fue crear el paquete manualmente en la estructura del proyecto de Android Studio.

AppCompatActivity: Es la clase base recomendada para actividades en Android.

3. Archivo Nuevo: activity_scan.xml
Objetivo: Definir el layout para la ScanActivity.

Ubicación: sample > src > main > res > layout > activity_scan.xml (Crear este archivo)

Código Final:

XML

<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="com.dji.sampleV5.barcodescanner.ScanActivity">

    <ImageView
        android:id="@+id/imageView_photo"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:scaleType="centerCrop"
        android:background="#CCCCCC"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintDimensionRatio="H,16:9"
        app:layout_constraintVertical_bias="0"
        tools:srcCompat="@drawable/ic_launcher_background"
        android:contentDescription="@string/photo_display_area" />

    <TextView
        android:id="@+id/textView_result"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginStart="16dp"
        android:layout_marginEnd="16dp"
        android:layout_marginTop="16dp"
        android:text="@string/scan_result_placeholder"
        android:textSize="18sp"
        android:textStyle="bold"
        android:textColor="@android:color/black"
        app:layout_constraintTop_toBottomOf="@+id/imageView_photo"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent" />

    <Button
        android:id="@+id/button_share"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="24dp"
        android:text="@string/share_button_text"
        app:layout_constraintTop_toBottomOf="@+id/textView_result"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
Solución de Errores / Consideraciones:

Error: Al crear este archivo, los atributos tools:context, android:contentDescription, android:text (para TextView y Button) podían aparecer en rojo.

Solución: Para cada uno de estos errores, se utilizó Alt + Enter (o Option + Enter en Mac) para:

tools:context: Establece el contexto de diseño para la vista previa.

@string/photo_display_area, @string/scan_result_placeholder, @string/share_button_text: Se seleccionó "Extract string resource" para crear estas cadenas en res/values/strings.xml, como se hizo en activity_main.xml.

4. Archivo Modificado: activity_main.xml
Objetivo: Añadir el botón para lanzar la ScanActivity.

Ubicación: sample > src > main > res > layout > activity_main.xml

Cambios Realizados (Código Final del Botón):

Dentro del LinearLayout del ScrollView (view_case_panel), después del testing_tool_button, se añadió el siguiente Button:

XML

                <Button
                    android:id="@+id/button_open_barcode_scanner"
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:elevation="10dp"
                    android:foreground="?selectableItemBackground"
                    android:layout_marginLeft="8dp"
                    android:layout_marginTop="8dp"
                    android:layout_marginRight="8dp"
                    android:background="@color/white"
                    android:enabled="true"
                    android:gravity="center"
                    android:padding="8dp"
                    android:text="@string/button_open_barcode_scanner_text"
                    android:textColor="@color/selector_enable_button" />
Solución de Errores / Consideraciones:

Error inicial: El usuario había pegado el código completo de activity_main.xml dentro de sí mismo, causando IDs duplicadas y errores de XML.

Solución: Se corrigió el problema proporcionando el código activity_main.xml completo y correcto, donde solo se añadió el nuevo botón en la ubicación adecuada, sin duplicar todo el layout.

Error: @string/button_open_barcode_scanner_text aparecía en rojo.

Solución: Se utilizó Alt + Enter sobre el texto en rojo y se seleccionó "Extract string resource". En el diálogo emergente, se estableció el "Resource name" como button_open_barcode_scanner_text y el "Resource value" como Abrir Lector de Barcodes.

5. Archivo Modificado: res/values/strings.xml
Objetivo: Almacenar los recursos de cadena para textos de la UI.

Ubicación: sample > src > main > res > values > strings.xml

Cambios Realizados (Ejemplo de Añadidos):

Las siguientes líneas se añaden automáticamente al usar "Extract string resource" en los pasos anteriores:

XML

    <string name="photo_display_area">Área de visualización de fotos</string>
    <string name="scan_result_placeholder">Resultado del escaneo aparecerá aquí</string>
    <string name="share_button_text">Compartir Resultado</string>
    <string name="button_open_barcode_scanner_text">Abrir Lector de Barcodes</string>
Solución de Errores / Consideraciones:

Este archivo se actualiza automáticamente. No es necesario editarlo manualmente para estas cadenas.

6. Archivo Modificado: DJIMainActivity.kt
Objetivo: Añadir la funcionalidad genérica para habilitar el nuevo botón del escáner.

Ubicación: sample > kotlin+java > dji.sampleV5.aircraft > DJIMainActivity.kt

Cambios Realizados (Código Final del Método):

Añade este nuevo método dentro de la clase DJIMainActivity:

Kotlin

    // ... otros métodos enable...

    fun <T> enableBarcodeScanner(cl: Class<T>) {
        enableShowCaseButton(binding.buttonOpenBarcodeScanner, cl)
    }

    // ... resto de la clase ...
Solución de Errores / Consideraciones:

Este método es una extensión del patrón ya existente en la clase para habilitar otros botones, aprovechando la función enableShowCaseButton.

7. Archivo Modificado: DJIAircraftMainActivity.kt
Objetivo: Lanzar la ScanActivity al hacer clic en el nuevo botón.

Ubicación: sample > kotlin+java > dji.sampleV5.aircraft > DJIAircraftMainActivity.kt

Cambios Realizados (Código Final del Método):

Añade la siguiente importación al inicio del archivo:

Kotlin

import com.dji.sampleV5.barcodescanner.ScanActivity
Modifica el método prepareTestingToolsActivity() para que quede así:

Kotlin

class DJIAircraftMainActivity : DJIMainActivity() {

    override fun prepareUxActivity() {
        UxSharedPreferencesUtil.initialize(this)
        GlobalPreferencesManager.initialize(DefaultGlobalPreferences(this))
        GeoidManager.getInstance().init(this)

        enableDefaultLayout(DefaultLayoutActivity::class.java)
        enableWidgetList(WidgetsActivity::class.java)
    }

    override fun prepareTestingToolsActivity() {
        enableTestingTools(AircraftTestingToolsActivity::class.java)
        enableBarcodeScanner(ScanActivity::class.java) // <-- ¡Esta es la línea clave!
    }
}
Solución de Errores / Consideraciones:

Error: Inicialmente hubo confusión sobre si modificar DJIMainActivity.kt o DJIAircraftMainActivity.kt para la llamada final.

Solución: Se determinó que DJIAircraftMainActivity.kt es la clase concreta que implementa prepareTestingToolsActivity(), por lo que la llamada a enableBarcodeScanner() debe ir aquí.

Importación: Asegurarse de importar ScanActivity para que el compilador sepa a qué clase se refiere ScanActivity::class.java.

Con este documento, tienes un registro completo de los cambios realizados y los problemas resueltos durante el proceso. ¡Espero que te sea muy útil como referencia!


Fuentes











Deep Research

Canvas

Imagen

