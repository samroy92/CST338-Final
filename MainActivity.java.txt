package com.example.papacasper.cst_final;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.AdapterView;
import android.widget.Button;
import android.widget.Spinner;
import android.widget.TextView;
import android.widget.Toast;
import java.lang.String;
import android.widget.ProgressBar;
import java.text.DecimalFormat;


/*
    Author: Sam Roy
    Project: CST-Final

    Description: The main point of this project was to learn and implement how to create a real time
    updating program that you can modify parameters and see the changes live. In this case we take
    a customization program of a car, and we see the results live as the user selects each option.
 */


//Main activity
public class MainActivity extends AppCompatActivity {

    //creating private class variables
    private Spinner spinnerEngine, spinnerTransmission, spinnerColor, spinnerEngineAccessories, spinnerTransmissionAccessories, spinnerWeight;
    private TextView viewText;
    private ProgressBar powerBar;
    private Button btnSubmit;
    private int horsePower;
    private int weight;
    private double zeroToSixty;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        //executing Listener functions
        spinnerEngineListener();
        spinnerEngineAccessoriesListener();
        spinnerTransmissionListener();
        spinnerTransmissionAccessoriesListener();
        spinnerWeightListener();
        addListenerOnButton();
    }


    //Listener for Engine dropdown
    public void spinnerEngineListener() {

        //Create new spinner object
        spinnerEngine = (Spinner)findViewById(R.id.spinnerEngine);

        //Call Java's built in Selected item listener to recalculate car
        spinnerEngine.setOnItemSelectedListener(new AdapterView.OnItemSelectedListener() {
            @Override
            public void onItemSelected(AdapterView<?> parent, View view, int position, long id) {
                calculateCar();
            }
            @Override
            public void onNothingSelected(AdapterView<?> parent) {
            }
        });
     }

    public void spinnerTransmissionListener() {

        //Create new spinner object
        spinnerTransmission = (Spinner)findViewById(R.id.spinnerTransmission);

        //Call Java's built in Selected item listener to recalculate car
        spinnerTransmission.setOnItemSelectedListener(new AdapterView.OnItemSelectedListener() {
            @Override
            public void onItemSelected(AdapterView<?> parent, View view, int position, long id) {
                calculateCar();
            }

            @Override
            public void onNothingSelected(AdapterView<?> parent) {

            }
        });
    }

    public void spinnerEngineAccessoriesListener() {

        //Create new spinner Object
        spinnerEngineAccessories = (Spinner)findViewById(R.id.spinnerEngineAccessories);

        //Call Java's built in Selected item listener to recalculate Car
        spinnerEngineAccessories.setOnItemSelectedListener(new AdapterView.OnItemSelectedListener() {
            @Override
            public void onItemSelected(AdapterView<?> parent, View view, int position, long id) {
                calculateCar();
            }

            @Override
            public void onNothingSelected(AdapterView<?> parent) {

            }
        });
    }

    public void spinnerTransmissionAccessoriesListener() {

        //Create new spinner Object
        spinnerTransmissionAccessories = (Spinner)findViewById(R.id.spinnerTransmissionAccessories);

        //Call Java's built in Selected item listener to recalculate Car
        spinnerTransmissionAccessories.setOnItemSelectedListener(new AdapterView.OnItemSelectedListener() {
            @Override
            public void onItemSelected(AdapterView<?> parent, View view, int position, long id) {
                calculateCar();
            }

            @Override
            public void onNothingSelected(AdapterView<?> parent) {

            }
        });
    }

    public void spinnerWeightListener() {

        //Create new spinner object
        spinnerWeight = (Spinner)findViewById(R.id.spinnerWeight);

        //Call Java's built in Selected item listener to recalculate car
        spinnerWeight.setOnItemSelectedListener(new AdapterView.OnItemSelectedListener() {
            @Override
            public void onItemSelected(AdapterView<?> parent, View view, int position, long id) {
                calculateCar();
            }

            @Override
            public void onNothingSelected(AdapterView<?> parent) {

            }
        });
    }

    /*
    Main function to calculate car values based on selections in menu
     */
    public void calculateCar() {

        //reset variables to defaults so changes are not additive, but replaced every time
        horsePower = 100;
        weight = 2500;
        zeroToSixty = 12.0;

        //Initialize String variables and assign values of current spinner selections are
        String selectedEngine = String.valueOf(spinnerEngine.getSelectedItem());
        String selectedTransmission = String.valueOf(spinnerTransmission.getSelectedItem());
        String selectedEngineExtras = String.valueOf(spinnerEngineAccessories.getSelectedItem());
        String selectedTransmissionExtras = String.valueOf(spinnerTransmissionAccessories.getSelectedItem());
        String selectedWeight = String.valueOf(spinnerWeight.getSelectedItem());


        //Switch statement to determine horsepower values
        switch(selectedEngine) {
            case "1.4L 4cyl": horsePower = horsePower + 10;
                break;
            case "2.0L 4cyl": horsePower = horsePower + 50;
                break;
            case "2.5L 4cyl": horsePower = horsePower + 75;
                break;
            case "3.0L 6cyl": horsePower = horsePower + 150;
                break;
            case "4.0L 6cyl": horsePower = horsePower + 200;
                break;
            case "6.2L 8cyl": horsePower = horsePower + 300;
                break;
            default: horsePower = 100;
                break;

        }

        //Switch statement to change weight based on transmission
        switch (selectedTransmission) {
            case "6-speed Manual": weight = weight - 200;
                break;
            case "7-speed Automatic": weight = weight + 100;
                break;
        }

        //Switch statement to change horsepower based on turbocharger
        switch (selectedEngineExtras) {
            case "Turbocharger": horsePower = horsePower + 100;
                break;
            case "Twin Turbo": horsePower = horsePower + 180;
                break;
            default: horsePower = horsePower;
        }

        //Switch statement to change weight based on shifter type
        switch (selectedTransmissionExtras) {
            case "Short Shifter": weight = weight - 50;
                break;
            case "Hydraulic Shifter": weight = weight - 100;
                break;
            default: weight = weight;
                break;
        }

        //Switch statement to change weight based on seat choices
        switch (selectedWeight) {
            case "Remove seats": weight = weight - 500;
                break;
            default: weight = weight;
        }

        //create double variables to determine power percentage
        double dHorsePower = ((double)horsePower / 580) * 100;

        //create double variable to determine zero-to-sixty gains/losses
        double dWeight = ((double)weight) / 1000;

        //create Decimalformat object to format output decimals
        DecimalFormat numberFormat = new DecimalFormat("##.00");

        //calculate zero-to-sixty time based on engine power and weight
        zeroToSixty = (zeroToSixty - (dHorsePower / 10)) + dWeight;


        //declare viewText variables to change field values
        viewText = (TextView) findViewById(R.id.textHorsepower);
        viewText.setText(String.valueOf(horsePower) + "HP");

        viewText = (TextView) findViewById(R.id.textWeightValue);
        viewText.setText(String.valueOf(weight) + "lbs");

        viewText = (TextView) findViewById(R.id.textZeroSityValue);
        viewText.setText(String.valueOf(numberFormat.format(zeroToSixty)) + " sec");

        //set progress bar to the amount of power specified
        powerBar = (ProgressBar) findViewById(R.id.progressPowerMeter);

        //requires int, have to typecast from double
        powerBar.setProgress((int)dHorsePower);

    }


    //Listener for Submit button
    public void addListenerOnButton() {

        //creating selected item variables
        spinnerEngine = (Spinner) findViewById(R.id.spinnerEngine);
        spinnerTransmission = (Spinner) findViewById(R.id.spinnerTransmission);
        spinnerColor = (Spinner) findViewById(R.id.spinnerColor);
        spinnerEngineAccessories = (Spinner) findViewById(R.id.spinnerEngineAccessories);
        spinnerTransmissionAccessories = (Spinner) findViewById(R.id.spinnerTransmissionAccessories);
        spinnerWeight = (Spinner) findViewById(R.id.spinnerWeight);

        //declaring new button object
        btnSubmit = (Button) findViewById(R.id.buttonSubmit);

        //main listener via OnClick
        btnSubmit.setOnClickListener(new View.OnClickListener() {

            public void onClick(View v) {

                //Simple toast output of selected choices
                Toast.makeText(MainActivity.this,
                                "Engine: "+ String.valueOf(spinnerEngine.getSelectedItem()) +
                                "\nTransmission: " + String.valueOf(spinnerTransmission.getSelectedItem()) +
                                "\nColor: " + String.valueOf(spinnerColor.getSelectedItem()) +
                                "\nEngine Extras: " + String.valueOf(spinnerEngineAccessories.getSelectedItem()) +
                                "\nTransmission Extras: " + String.valueOf(spinnerTransmissionAccessories.getSelectedItem()) +
                                "\nWeight Reduction: " + String.valueOf(spinnerWeight.getSelectedItem()),
                        Toast.LENGTH_SHORT).show();

            }

        });
    }
}


