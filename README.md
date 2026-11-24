## EX:NO:05:Develop a program to create a simple calculator using android studio.
## Aim:
To create and design an android application for a simple calculator using android studio.
## EQUIPMENTS REQUIRED:
Android Studio(Latest Version)
## ALGORITHM:
Step 1: Open Android Stdio and then click on File -> New -> New project.

Step 2: Then type the Application name as smsintent and click Next.

Step 3: Then select the Minimum SDK as shown below and click Next.

Step 4: Then select the Empty Activity and click Next. Finally click Finish.

Step 5: Design layout in activity_main.xml.

Step 6:Display details give in MainActivity file.

Step 7: Save and run the application.
## PROGRAM:
Program to create and design an android application simple calculator using Intent.
### Developed by: MAHALINGA JEYANTH V 
### Registeration Number :212224220057

## AndroidMainfest.xml
```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.SimpleCalculator">
        <activity
            android:name=".MainActivity"
            android:exported="true"
            android:windowSoftInputMode="adjustResize">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```
## MainActivity.java
```
package com.example.simplecalculator;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;

import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    private TextView tvExpression, tvResult;

    private String currentInput = "";   // number being typed
    private double firstNumber = 0;     // stored left operand
    private String operator = "";       // + - × ÷
    private boolean resultShown = false;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        tvExpression = findViewById(R.id.tvExpression);
        tvResult = findViewById(R.id.tvResult);

        // Number buttons 0-9
        int[] numberIds = {
                R.id.btn0, R.id.btn1, R.id.btn2, R.id.btn3, R.id.btn4,
                R.id.btn5, R.id.btn6, R.id.btn7, R.id.btn8, R.id.btn9
        };
        View.OnClickListener numberListener = v ->
                appendDigit(((Button) v).getText().toString());
        for (int id : numberIds) {
            findViewById(id).setOnClickListener(numberListener);
        }

        // Operator buttons
        int[] operatorIds = {
                R.id.btnAdd, R.id.btnSubtract, R.id.btnMultiply, R.id.btnDivide
        };
        View.OnClickListener operatorListener = v ->
                setOperator(((Button) v).getText().toString());
        for (int id : operatorIds) {
            findViewById(id).setOnClickListener(operatorListener);
        }

        // Other buttons
        findViewById(R.id.btnDot).setOnClickListener(v -> appendDot());
        findViewById(R.id.btnEquals).setOnClickListener(v -> calculate());
        findViewById(R.id.btnClear).setOnClickListener(v -> clearAll());
        findViewById(R.id.btnDelete).setOnClickListener(v -> deleteLast());
        findViewById(R.id.btnPercent).setOnClickListener(v -> percent());
    }

    private void appendDigit(String digit) {
        if (resultShown) {          // start fresh after "="
            clearAll();
        }
        if ("0".equals(currentInput)) {
            currentInput = digit;   // avoid leading zeros like 007
        } else {
            currentInput += digit;
        }
        tvResult.setText(currentInput);
    }

    private void appendDot() {
        if (resultShown) {
            clearAll();
        }
        if (currentInput.contains(".")) return;
        if (currentInput.isEmpty()) currentInput = "0";
        currentInput += ".";
        tvResult.setText(currentInput);
    }

    private void setOperator(String op) {
        // allow continuing from a previous result
        if (resultShown) {
            resultShown = false;
        }
        // chain: 5 + 3 + ... computes 5 + 3 first
        if (!operator.isEmpty() && !currentInput.isEmpty()) {
            calculate();
            resultShown = false;
        }
        if (!currentInput.isEmpty()) {
            firstNumber = Double.parseDouble(currentInput);
        } else if (operator.isEmpty()) {
            firstNumber = 0;
        }
        operator = op;
        currentInput = "";
        tvExpression.setText(getString(R.string.expression_op, format(firstNumber), operator));
    }

    private void calculate() {
        if (operator.isEmpty() || currentInput.isEmpty()) return;

        double secondNumber = Double.parseDouble(currentInput);
        double result;

        switch (operator) {
            case "+":
                result = firstNumber + secondNumber;
                break;
            case "−":
                result = firstNumber - secondNumber;
                break;
            case "×":
                result = firstNumber * secondNumber;
                break;
            case "÷":
                if (secondNumber == 0) {
                    tvExpression.setText("");
                    tvResult.setText(R.string.cannot_divide_by_zero);
                    resetState();
                    return;
                }
                result = firstNumber / secondNumber;
                break;
            default:
                return;
        }

        tvExpression.setText(getString(R.string.expression_equals, format(firstNumber), operator, format(secondNumber)));
        tvResult.setText(format(result));

        currentInput = format(result);  // lets user continue with the result
        firstNumber = result;
        operator = "";
        resultShown = true;
    }

    private void percent() {
        if (currentInput.isEmpty()) return;
        double value = Double.parseDouble(currentInput) / 100;
        currentInput = format(value);
        tvResult.setText(currentInput);
    }

    private void deleteLast() {
        if (resultShown || currentInput.isEmpty()) return;
        currentInput = currentInput.substring(0, currentInput.length() - 1);
        tvResult.setText(currentInput.isEmpty() ? "0" : currentInput);
    }

    private void clearAll() {
        resetState();
        tvExpression.setText("");
        tvResult.setText("0");
    }

    private void resetState() {
        currentInput = "";
        firstNumber = 0;
        operator = "";
        resultShown = false;
    }

    // Show 5.0 as "5", keep 2.5 as "2.5"
    private String format(double value) {
        if (value == (long) value) {
            return Long.toString((long) value);
        }
        return String.valueOf(value);
    }
}
```
# activitymain.xml
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:background="#212121"
    android:padding="12dp">

    <!-- Display area -->
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="0.8"
        android:gravity="bottom|end"
        android:orientation="vertical"
        android:padding="8dp">

        <TextView
            android:id="@+id/tvExpression"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:gravity="end"
            android:text=""
            android:textColor="#B0BEC5"
            android:textSize="24sp" />

        <TextView
            android:id="@+id/tvResult"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:gravity="end"
            android:maxLines="1"
            android:text="@string/_0"
            android:textColor="#FFFFFF"
            android:textSize="56sp" />
    </LinearLayout>

    <!-- Buttons -->
    <GridLayout
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="2"
        android:columnCount="4"
        android:rowCount="5">

        <Button android:id="@+id/btnClear"   style="@style/CalcClear"    android:text="@string/clear" />
        <Button android:id="@+id/btnDelete"  style="@style/CalcOperator" android:text="@string/delete" />
        <Button android:id="@+id/btnPercent" style="@style/CalcOperator" android:text="@string/percent" />
        <Button android:id="@+id/btnDivide"  style="@style/CalcOperator" android:text="@string/divide" />

        <Button android:id="@+id/btn7" style="@style/CalcButton" android:text="@string/num_7" />
        <Button android:id="@+id/btn8" style="@style/CalcButton" android:text="@string/num_8" />
        <Button android:id="@+id/btn9" style="@style/CalcButton" android:text="@string/num_9" />
        <Button android:id="@+id/btnMultiply" style="@style/CalcOperator" android:text="@string/multiply" />

        <Button android:id="@+id/btn4" style="@style/CalcButton" android:text="@string/num_4" />
        <Button android:id="@+id/btn5" style="@style/CalcButton" android:text="@string/num_5" />
        <Button android:id="@+id/btn6" style="@style/CalcButton" android:text="@string/num_6" />
        <Button android:id="@+id/btnSubtract" style="@style/CalcOperator" android:text="@string/subtract" />

        <Button android:id="@+id/btn1" style="@style/CalcButton" android:text="@string/num_1" />
        <Button android:id="@+id/btn2" style="@style/CalcButton" android:text="@string/num_2" />
        <Button android:id="@+id/btn3" style="@style/CalcButton" android:text="@string/num_3" />
        <Button android:id="@+id/btnAdd" style="@style/CalcOperator" android:text="@string/add" />

        <Button android:id="@+id/btn0" style="@style/CalcButton"
            android:layout_columnSpan="2" android:text="@string/_0" />
        <Button android:id="@+id/btnDot" style="@style/CalcButton" android:text="@string/dot" />
        <Button android:id="@+id/btnEquals" style="@style/CalcOperator" android:text="@string/equals" />
    </GridLayout>
</LinearLayout>
```
### output
UI design part:
<img width="1920" height="1200" alt="Screenshot (58)" src="https://github.com/user-attachments/assets/b89a7d1b-b171-4433-b04d-83bbf803f859" />

output:
<img width="1920" height="1200" alt="Screenshot (60)" src="https://github.com/user-attachments/assets/bb0b23e3-fa9f-4a44-897c-4620277c8744" />


## RESULT
Thus a Simple Android Application create a simple calculator using Android Studio is developed and executed successfully.
