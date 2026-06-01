# DM-Lab-21-Capteurs-embarqu-s-Android

Objectif général
Ce lab a pour objectif de développer progressivement une application Android permettant d’exploiter les capteurs embarqués d’un smartphone. L’application doit afficher les capteurs disponibles, lire leurs caractéristiques techniques, visualiser les mesures sous forme de graphes et exploiter les capteurs de mouvement pour proposer une reconnaissance simple d’activité.Compétences visées
À la fin du lab, l’étudiant doit être capable de :
Comprendre le rôle de SensorManager dans Android.
Lister les capteurs disponibles dans un dispositif.
Lire les propriétés techniques d’un capteur.
Exploiter SensorEventListener pour recevoir des mesures en temps réel.
Afficher l’évolution d’un capteur sous forme de graphe.
Utiliser l’accéléromètre, le gyroscope, le magnétomètre, le capteur de proximité et le compteur de pas.
Mettre en place une logique simple de reconnaissance d’activité.



Objectif pédagogique
Cette partie permet d’afficher la liste complète des capteurs disponibles sur le téléphone ou l’émulateur. Pour chaque capteur, les informations techniques demandées dans le TP doivent être ajoutées :
Résolution
Besoins en énergie
Maximum Range
Int Type
Vitesse maximale d’acquisition
Uploaded Image
Étape 1 — Créer la classe SensorFormatter
Créer le fichier :
utils/SensorFormatter.java
package com.example.sensors.utils;

import android.hardware.Sensor;

public class SensorFormatter {

    public static String format(Sensor sensor) {
        return "Id : " + sensor.getId() + "\n"
                + "Name : " + sensor.getName() + "\n"
                + "Vendor : " + sensor.getVendor() + "\n"
                + "Version : " + sensor.getVersion() + "\n"
                + "Type : " + sensor.getStringType() + "\n"
                + "Int Type : " + sensor.getType() + "\n"
                + "Resolution : " + sensor.getResolution() + "\n"
                + "Power : " + sensor.getPower() + " mA\n"
                + "Maximum Range : " + sensor.getMaximumRange() + "\n"
                + "Min Delay : " + sensor.getMinDelay() + " µs\n";
    }
}
Explication du code
La classe SensorFormatter transforme un objet Sensor en texte lisible.
getName() retourne le nom du capteur.
getVendor() retourne le fabricant.
getVersion() retourne la version du capteur.
getStringType() retourne le type textuel du capteur.
getType() retourne le type entier du capteur. C’est l’information demandée dans le TP sous le nom Int Type.
getResolution() retourne la résolution du capteur.
getPower() retourne la consommation énergétique du capteur en milliampères.
getMaximumRange() retourne la valeur maximale que le capteur peut mesurer.
getMinDelay() retourne le délai minimal entre deux acquisitions. Cette valeur permet d’estimer la vitesse maximale d’acquisition.


Étape 2 — Créer le fragment SensorsListFragment
package com.example.sensors.fragments;

import android.content.Context;
import android.hardware.Sensor;
import android.hardware.SensorManager;
import android.os.Bundle;
import android.view.View;
import android.view.ViewGroup;
import android.view.LayoutInflater;
import android.widget.LinearLayout;
import android.widget.ScrollView;
import android.widget.TextView;

import androidx.annotation.NonNull;
import androidx.annotation.Nullable;
import androidx.fragment.app.Fragment;

import com.example.sensors.utils.SensorFormatter;

import java.util.List;

public class SensorsListFragment extends Fragment {

    private SensorManager sensorManager;
    private LinearLayout container;

    @Nullable
    @Override
    public View onCreateView(
            @NonNull LayoutInflater inflater,
            @Nullable ViewGroup parent,
            @Nullable Bundle savedInstanceState) {

        ScrollView scrollView = new ScrollView(requireContext());

        container = new LinearLayout(requireContext());
        container.setOrientation(LinearLayout.VERTICAL);
        container.setPadding(24, 24, 24, 24);

        scrollView.addView(container);

        sensorManager = (SensorManager)
                requireActivity().getSystemService(Context.SENSOR_SERVICE);

        displaySensors();

        return scrollView;
    }

    private void displaySensors() {
        List<Sensor> sensors = sensorManager.getSensorList(Sensor.TYPE_ALL);

        for (Sensor sensor : sensors) {
            TextView textView = new TextView(requireContext());
            textView.setText(SensorFormatter.format(sensor));
            textView.setTextSize(14);
            textView.setPadding(16, 16, 16, 16);

            container.addView(textView);

            View separator = new View(requireContext());
            separator.setLayoutParams(
                    new LinearLayout.LayoutParams(
                            ViewGroup.LayoutParams.MATCH_PARENT,
                            2));

            separator.setBackgroundColor(0xFFE0E0E0);
            container.addView(separator);
        }
    }
}
Explication du code
SensorManager est le service Android qui permet d’accéder aux capteurs.
getSensorList(Sensor.TYPE_ALL) récupère tous les capteurs disponibles.
Chaque capteur est affiché dans un TextView.
Le ScrollView permet de faire défiler la liste lorsque le téléphone contient beaucoup de capteurs.
Le séparateur gris améliore la lisibilité de l’affichage
