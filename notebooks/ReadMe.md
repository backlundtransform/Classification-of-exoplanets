# README – Exoplanet Database Updater

This project contains a Python script that imports exoplanet data from a CSV file and updates a SQLite database (`exo.db`). The purpose of the script is to automate synchronization of star and exoplanet information from an external dataset.

## ⭐ Overview

The script performs the following actions:

### 1. Reads CSV Input

* Loads `hwc.csv` using **pandas**.
* Each row represents a planet and its host star.

### 2. Connects to the SQLite Database

* Opens (or creates) the database `exo.db` using `sqlite3`.

### 3. Updates Existing Planets

For each row in the CSV, the script checks if the planet already exists in the **Planets** table:

* If the planet **exists**, an `UPDATE` query is executed to refresh all relevant fields:

  * Planet classifications
  * Physical parameters (mass, radius, density, gravity, escape velocity)
  * Stellar flux values
  * Equilibrium and surface temperature estimates
  * Orbital parameters
  * Discovery information
  * Habitability flag

### 4. Inserts New Stars

If the planet does not exist, the script checks whether its host star is already present in the **Stars** table:

* If the star **does not exist**, a new entry is inserted, including:

  * Spectral type
  * Mass, radius, temperature
  * Luminosity and age
  * Magnitude
  * Coordinates (RA, Dec)
  * Habitable zone range
  * Associated constellation

### 5. Inserts New Planets

After ensuring the star exists, the script inserts a new record into **Planets**, including:

* Classification data
* Physical attributes
* Flux and temperature metrics
* Orbital characteristics
* ESI
* Habitability
* Discovery method and year
* Foreign key: `StarId`

### 6. Commits All Changes

* Each update or insert is committed using `con.commit()`.

### 7. Closes the Database Connection

* After processing all rows, the connection is closed with `con.close()`.

---

## 📁 File Structure

Example repository layout:

```
/repo-root
│
├── hwc.csv                  # Input dataset with star and planet data
├── exo.db                   # SQLite database
├── update_exoplanets.py     # The script (your code)
└── README.md
```

---

## 🧩 Requirements

The script uses the following Python libraries:

* `pandas`
* `sqlite3` (built-in)

Install dependencies (if using a requirements file):

```
pip install -r requirements.txt
```

Or just:

```
pip install pandas
```

---

## ▶️ How to Run

Run the script from the command line:

```
python update_exoplanets.py
```

Prerequisites:

* `hwc.csv` must exist in the same directory as the script.
* The SQLite database `exo.db` must contain the tables **Stars**, **Planets**, and **Constellations**, with schemas compatible with the fields used in the script.

---

## 📝 Database Tables (Summary)

### **Stars**

Fields read or inserted include:

* Name
* Type
* Mass, Radius
* Effective Temperature
* Luminosity
* Age
* Apparent Magnitude
* Distance
* Right Ascension, Declination
* Habitable Zone Min/Max
* ConstellationId

### **Planets**

Fields updated or inserted:

* Name
* ZoneClass, MassClass, AtmosphereClass
* Mass, Radius, Density
* Gravity, Escape Velocity
* Stellar Flux (Min/Mean/Max)
* Equilibrium Temperature (Min/Mean/Max)
* Surface Temperature (Min/Mean/Max)
* Orbital Period
* Semimajor Axis
* Eccentricity
* Mean Distance
* Inclination
* Omega
* ESI
* Habitable (boolean)
* Discovery Method
* Discovery Year
* StarId

---

## 🔍 Purpose

This script serves as a **data synchronization tool** for maintaining an up-to-date database of exoplanets and their host stars.
It is intended to support projects such as scientific visualization, e.g. rendering realistic exoplanetary systems in **React / Three.js**.

