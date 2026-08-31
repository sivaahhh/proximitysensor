# Ex.No:5 Develop a simple application for proximity sensor using Sensor Manager in android studio.


## AIM:

To develop a sensor application for proximity sensor using sensor manager in Android Studio.

## EQUIPMENTS REQUIRED:

Android Studio(Min.required Giraffe)

## ALGORITHM:

Step 1: Open Android Stdio and then click on File -> New -> New project.

Step 2: Then type the Application name as proximitysensor and click Next. 

Step 3: Then select the Minimum SDK as shown below and click Next.

Step 4: Then select the Empty Activity and click Next. Finally click Finish.

Step 5: Design layout in activity_main.xml.

Step 6: Display process of proximitysensor in android mobile devices.

Step 7: Save and run the application.

## PROGRAM:
```
/*
Program to print the process of proximitysensor in android mobile devices”.
Developed by: SIVA SAKTHI A
Registeration Number : 212224223005
*/
```
## Main Activity:
```
package com.example.proximitysensor;

import android.content.Context;
import android.graphics.Color;
import android.hardware.Sensor;
import android.hardware.SensorEvent;
import android.hardware.SensorEventListener;
import android.hardware.SensorManager;
import android.os.Bundle;
import android.view.View;
import android.widget.TextView;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

import java.util.Locale;

public class MainActivity extends AppCompatActivity implements SensorEventListener {

    private SensorManager sensorManager;
    private Sensor proximitySensor;
    private TextView proximityStatus;
    private TextView distanceText;
    private TextView maxRangeText;
    private TextView sensorAvailableText;
    private View circleContainer;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);
        
        proximityStatus = findViewById(R.id.proximityStatus);
        distanceText = findViewById(R.id.distanceText);
        maxRangeText = findViewById(R.id.maxRangeText);
        sensorAvailableText = findViewById(R.id.sensorAvailableText);
        circleContainer = findViewById(R.id.circleContainer);
        
        sensorManager = (SensorManager) getSystemService(Context.SENSOR_SERVICE);
        if (sensorManager != null) {
            proximitySensor = sensorManager.getDefaultSensor(Sensor.TYPE_PROXIMITY);
        }

        if (proximitySensor != null) {
            sensorAvailableText.setText("Sensor: Available ✓");
            maxRangeText.setText(String.format(Locale.getDefault(), "Max Range: %.1f cm", proximitySensor.getMaximumRange()));
        } else {
            sensorAvailableText.setText("Sensor: Not Available ✗");
            maxRangeText.setText("Max Range: N/A");
            proximityStatus.setText("N/A");
            circleContainer.setVisibility(View.INVISIBLE);
        }

        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main), (v, insets) -> {
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
            return insets;
        });
    }

    @Override
    protected void onResume() {
        super.onResume();
        if (proximitySensor != null) {
            sensorManager.registerListener(this, proximitySensor, SensorManager.SENSOR_DELAY_NORMAL);
        }
    }

    @Override
    protected void onPause() {
        super.onPause();
        if (proximitySensor != null) {
            sensorManager.unregisterListener(this);
        }
    }

    @Override
    public void onSensorChanged(SensorEvent event) {
        if (event.sensor.getType() == Sensor.TYPE_PROXIMITY) {
            float distance = event.values[0];
            distanceText.setText(String.format(Locale.getDefault(), "Distance: %.1f cm", distance));
            
            if (distance < proximitySensor.getMaximumRange()) {
                proximityStatus.setText("NEAR");
                proximityStatus.setTextColor(Color.RED);
            } else {
                proximityStatus.setText("FAR");
                proximityStatus.setTextColor(Color.WHITE);
            }
        }
    }

    @Override
    public void onAccuracyChanged(Sensor sensor, int accuracy) {
        // Not used
    }
}
```
## ActivityMain.XML:
```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#121212"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/title"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="64dp"
        android:text="Proximity Sensor"
        android:textColor="#4CAF50"
        android:textSize="28sp"
        android:textStyle="bold"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <FrameLayout
        android:id="@+id/circleContainer"
        android:layout_width="250dp"
        android:layout_height="250dp"
        android:background="@drawable/circle_shape"
        app:layout_constraintBottom_toTopOf="@+id/distanceText"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/title">

        <TextView
            android:id="@+id/proximityStatus"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="center"
            android:text="NEAR"
            android:textColor="#FF0000"
            android:textSize="32sp"
            android:textStyle="bold" />
    </FrameLayout>

    <TextView
        android:id="@+id/distanceText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="16dp"
        android:text="Distance: 0.0 cm"
        android:textColor="#FFFFFF"
        android:textSize="18sp"
        app:layout_constraintBottom_toTopOf="@+id/maxRangeText"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent" />

    <TextView
        android:id="@+id/maxRangeText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="32dp"
        android:text="Max Range: 0.0 cm"
        android:textColor="#FFFFFF"
        android:textSize="18sp"
        app:layout_constraintBottom_toTopOf="@+id/sensorAvailableText"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent" />

    <TextView
        android:id="@+id/sensorAvailableText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="64dp"
        android:text="Sensor: Available ✓"
        android:textColor="#AAAAAA"
        android:textSize="14sp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
```
## OUTPUT
## Proximity Sensor FAR
<img width="1920" height="1080" alt="pmd ex5 far" src="https://github.com/user-attachments/assets/93053223-2c05-4731-a5a1-54526b919dbb" />

## Proximity Sensor NEAR
<img width="1920" height="1080" alt="pmd ex5 near" src="https://github.com/user-attachments/assets/23439c9c-7508-4950-9bf9-1ae703747aae" />


## RESULT
Thus a Simple Android Application to display the details of proximity sensor using sensor manager in Android Studio is developed and executed successfully.
