# ProjetAR3
# README.md – TP Calculatrice Qt

## 🧪 TP C++ / Qt – Calculatrice orientée objet (Qt Widgets)

### 🎯 Objectifs pédagogiques

- Apprendre à structurer un projet C++ avec des **classes orientées objet**.
- Découvrir le développement **d’interfaces graphiques avec Qt Widgets**.
- Comprendre et utiliser les **signaux et slots** dans Qt.
- Développer une **calculatrice graphique** simple, modulaire et fonctionnelle.

---

### 🛠 Prérequis

- Connaissances de base en **C++** (classes, fonctions, pointeurs).
- Qt Creator installé avec **QQt 6.x** sur la machine virtuelle fournie (mdp isty).
- Base de l’interface utilisateur avec le designer `.ui`.

---

### 🧩 Architecture du projet

Le projet est composé de trois grandes parties :

| Classe | Rôle |
|--------|------|
| `Calculatrice` | Contient la logique métier des opérations mathématiques. |
| `FenetrePrincipale` | Gère l'interface utilisateur (hérite de `QMainWindow`). |
| `main.cpp` | Lance l'application Qt. |

---

### 📋 Instructions

#### 1. Création du projet

- Dans Qt Creator, créer un nouveau projet `Qt Widgets Application`.
- Nom du projet : `CalculatricePOO`.

#### 2. Conception de la classe `Calculatrice`

Créer un fichier `calculatrice.h` et `calculatrice.cpp` avec les méthodes :

- `addition(double a, double b)`
- `soustraction(double a, double b)`
- `multiplication(double a, double b)`
- `division(double a, double b)` (attention à la division par zéro !)

#### 3. Création de l'interface graphique

Via Qt Designer (`.ui`) :
- `QLineEdit` pour le premier nombre.
- `QLineEdit` pour le deuxième nombre.
- `QComboBox` pour choisir l’opération : `+`, `-`, `*`, `/`.
- `QPushButton` pour lancer le calcul.
- `QLabel` pour afficher le résultat.

#### 4. Liaison entre UI et calculs

- Dans `fenetreprincipale.cpp`, connecter le bouton au slot personnalisé `on_boutonCalculer_clicked()`.
- Lire les valeurs des champs, déterminer l'opération choisie, appeler la méthode correspondante de `Calculatrice` et afficher le résultat.

---

### ✳️ Extensions possibles

- Ajouter un historique des opérations dans un `QListWidget`.
- Gérer les erreurs (ex : texte invalide, division par zéro).
- Ajouter un mode scientifique (racine, puissance, etc.).
- Implémenter un contrôleur intermédiaire pour une architecture MVC.

---

### ✅ Critères de réussite

- Structure objet propre (au moins 1 classe pour la logique métier).
- Interface fonctionnelle, responsive.
- Code clair, documenté.
- Application exécutable sans erreur.

---

### 💾 Pour aller plus loin

- Remplacer Qt Widgets par Qt Quick/QML.
- Utiliser des tests unitaires avec `QTest`.
- Sauvegarder l’historique dans un fichier texte (bonus).

---

🧠 N’oubliez pas que ce TP vise autant la rigueur de conception que le résultat graphique. Travaillez en petits groupes, discutez de l’architecture avant de coder !

// calculatrice.h
#ifndef CALCULATRICE_H
#define CALCULATRICE_H

class Calculatrice {
public:
    double addition(double a, double b);
    double soustraction(double a, double b);
    double multiplication(double a, double b);
    double division(double a, double b);
};

#endif // CALCULATRICE_H


// calculatrice.cpp
#include "calculatrice.h"

double Calculatrice::addition(double a, double b) { return a + b; }
double Calculatrice::soustraction(double a, double b) { return a - b; }
double Calculatrice::multiplication(double a, double b) { return a * b; }
double Calculatrice::division(double a, double b) {
    if (b != 0) return a / b;
    return 0; // ou lancer une exception
}


// main.cpp
#include <QApplication>
#include "fenetreprincipale.h"

int main(int argc, char *argv[]) {
    QApplication app(argc, argv);
    FenetrePrincipale w;
    w.show();
    return app.exec();
}


// fenetreprincipale.h
#ifndef FENETREPRINCIPALE_H
#define FENETREPRINCIPALE_H

#include <QMainWindow>
#include "calculatrice.h"

QT_BEGIN_NAMESPACE
namespace Ui { class FenetrePrincipale; }
QT_END_NAMESPACE

class FenetrePrincipale : public QMainWindow {
    Q_OBJECT

public:
    FenetrePrincipale(QWidget *parent = nullptr);
    ~FenetrePrincipale();

private slots:
    void on_boutonCalculer_clicked();

private:
    Ui::FenetrePrincipale *ui;
    Calculatrice calc;
};

#endif // FENETREPRINCIPALE_H


// fenetreprincipale.cpp
#include "fenetreprincipale.h"
#include "ui_fenetreprincipale.h"

FenetrePrincipale::FenetrePrincipale(QWidget *parent)
    : QMainWindow(parent), ui(new Ui::FenetrePrincipale) {
    ui->setupUi(this);
    connect(ui->boutonCalculer, &QPushButton::clicked,
            this, &FenetrePrincipale::on_boutonCalculer_clicked);
}

FenetrePrincipale::~FenetrePrincipale() {
    delete ui;
}

void FenetrePrincipale::on_boutonCalculer_clicked() {
    bool ok1, ok2;
    double a = ui->lineEditNombre1->text().toDouble(&ok1);
    double b = ui->lineEditNombre2->text().toDouble(&ok2);
    QString operation = ui->comboBoxOperation->currentText();

    if (ok1 && ok2) {
        double resultat = 0;
        if (operation == "+") resultat = calc.addition(a, b);
        else if (operation == "-") resultat = calc.soustraction(a, b);
        else if (operation == "*") resultat = calc.multiplication(a, b);
        else if (operation == "/") resultat = calc.division(a, b);

        ui->labelResultat->setText("Résultat : " + QString::number(resultat));
    } else {
        ui->labelResultat->setText("Entrées invalides !");
    }
}
