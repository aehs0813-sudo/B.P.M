// Detector de BPM - Versión Manual 
// El programa permite ingresar un archivo .wav o .mp3
// y asignarle un BPM manualmente.

// --- Librerías ---
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
#include <limits>

using namespace std;

// ------------------------------------------------------------
// VARIABLES GLOBALES 
// ------------------------------------------------------------
bool programaActivo = true;
int opcion = 0;
int bpm = 0;
string rutaArchivo = "";
int resultadoDeteccion = 0;

// Constantes de BPM permitidas
const int BPM_MIN = 40;
const int BPM_MAX = 240;

// ------------------------------------------------------------
// Función: leerEnteroEnRango
// Explicación: Pide un número al usuario y valida que esté en un rango.
// ------------------------------------------------------------
int leerEnteroEnRango(const string& mensaje, int minimo, int maximo) {
    int valor = 0;

    while (true) { //Sigue pidiendo el valor hasta q sea valido
        cout << mensaje;

        if (cin >> valor && valor >= minimo && valor <= maximo) { //Intenta leer un número.
            break; // aceptado, sale del loop cua el v este in dl rang
        } else {
            cin.clear(); //Limpia el estado de error si el usuario escribe el txt.
            cin.ignore(numeric_limits<streamsize>::max(), '\n'); //limpia el buffer para evitar un loop infinito
            cout << "Número inválido. Debe estar entre " << minimo
                 << " y " << maximo << ".\n";
        }
    }

    return valor;
}

// ------------------------------------------------------------
// Función: detectarBPMConAubio 
// ------------------------------------------------------------
int detectarBPMConAubio(const string& ruta) {
    (void)ruta; // evita warning de variable no usada
    cout << "[Info] La detección automática REAL no está implementada.\n";
    return -1; // siempre falla (manual fallback)
}

// ------------------------------------------------------------
// Función: detectarBPM
// ------------------------------------------------------------
int detectarBPM(const string& ruta) {
    cout << "\n[Info] Intentando detectar BPM para: " << ruta << "\n";

    int autoBPM = detectarBPMConAubio(ruta);

    if (autoBPM < 0) {
        cout << "[Advertencia] Falló la detección automática.\n";
        autoBPM = leerEnteroEnRango("Ingrese BPM manualmente (40–240): ",
                                    BPM_MIN, BPM_MAX);
    }

    return autoBPM;
}

// ------------------------------------------------------------
// Función: mostrarMenu
// ------------------------------------------------------------
void mostrarMenu() {
    cout << "\n=== Detector de BPM (Versión Manual) ===\n"
         << "1) Detectar BPM de un archivo de audio\n"
         << "0) Salir\n> ";
}

// ------------------------------------------------------------
// Función principal
// ------------------------------------------------------------
int main() {

    while (programaActivo) {
        mostrarMenu();

        if (!(cin >> opcion)) {
            cin.clear();
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
            cout << "Entrada inválida.\n";
            continue;
        }

        if (opcion == 1) {
            cin.ignore(numeric_limits<streamsize>::max(), '\n'); // limpiar buffer

            cout << "Ingrese la ruta del archivo (.wav o .mp3): ";
            getline(cin, rutaArchivo);

            bpm = detectarBPM(rutaArchivo);

            cout << "\nResultado:\n";
            cout << "  Archivo : " << rutaArchivo << "\n";
            cout << "  BPM     : " << bpm << "\n";
        }
        else if (opcion == 0) {
            programaActivo = false;
        }
        else {
            cout << "Opción inválida.\n";
        }
    }

    cout << "¡Hasta luego!\n";
    return 0;  
}
