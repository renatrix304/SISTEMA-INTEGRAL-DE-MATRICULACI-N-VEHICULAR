PROGRAMACION 1
PROYECTO INTEGREADOR FINAL 

[Copia de SISTEMA INBTREGRAL DE MATRICULACIÓN VEHICULAR_compressed (1).pdf](https://github.com/user-attachments/files/20679378/Copia.de.SISTEMA.INBTREGRAL.DE.MATRICULACION.VEHICULAR_compressed.1.pdf)
#include <stdio.h>
#include <stdlib.h>
#include "RevisionVehicular.h"

void menuPrincipal();

int main() {
	menuPrincipal();
	return 0;
}

void menuPrincipal() {
	int opcion;
	printf("\n===== SISTEMA DE MATRICULACION VEHICULAR =====\n");
	printf("1. Registrar vehiculo\n");
	printf("2. Calcular valor matricula\n");
	printf("3. Registrar revisiones tecnicas\n");
	printf("4. Mostrar estado de revisiones\n");
	printf("5. Salir\n");
	printf("Seleccione una opcion: ");
	scanf("%d", &opcion);
	getchar(); 
	
	Vehiculo v; 
	
	switch (opcion) {
	case 1:
		validarMatricula(&v);
		break;
	case 2:
	{
		float total = calcular_matricula_vehicular("liviano", 1600, 2015, "Quito", 0, 1, 40.0, 4, 7);
		printf("Total matricula calculado: $%.2f\n", total);
	}
	break;
	case 3:
		registrarRevisiones(v.revisiones);
		break;
	case 4:
		mostrarEstadoRevisiones(v);
		break;
	case 5:
		printf("Saliendo del sistema...\n");
		exit(0);
	default:
		printf("Opción no valida. Intente nuevamente.\n");
	}
	
	printf("\nPresione Enter para continuar...");
	getchar();
	menuPrincipal(); 
}




#include <stdio.h>
#include <string.h>
#include "RevisionVehicular.h"

void validarMatricula(Vehiculo *v) {
	printf("\n--- REGISTRO DE VEHICULO ---\n");
	
	printf("Ingrese numero de placa: ");
	scanf("%s", v->placa);
	
	printf("Ingrese cédula del propietario: ");
	scanf("%s", v->cedula);
	
	printf("Ingrese año del vehiculo: ");
	scanf("%s", v->anio);
	
	printf("Ingrese tipo de vehiculo: ");
	scanf("%s", v->tipo);
	
	printf("Ingrese avaluo del vehiculo: ");
	scanf("%s", v->avaluo);
	
	// Inicializar revisiones como no cumplidas
	for (int i = 0; i < NUM_REVISIONES; i++) {
		v->revisiones[i] = 0;
	}
	
	printf("? Vehiculo registrado correctamente.\n");
}

float calcular_matricula_vehicular(const char tipo_vehiculo[], int cilindraje, int anio_fabricacion,
								   const char provincia[], float es_ecologico, float revision_obligatoria,
								   float multas_transito, int mes_pago, int ultimo_digito_placa) {
	float valor_total = 0.0;
	
	// Tipo de vehículo
	if (strcmp(tipo_vehiculo, "liviano") == 0) {
		valor_total += 30.0;
		if (cilindraje > 1500) {
			valor_total += (cilindraje - 1500) * 0.01;
		}
	} else if (strcmp(tipo_vehiculo, "moto") == 0) {
		if (cilindraje <= 250)
			valor_total += 5.0;
		else if (cilindraje <= 500)
			valor_total += 10.0;
		else
			valor_total += 20.0;
	} else if (strcmp(tipo_vehiculo, "pesado") == 0) {
		valor_total += 80.0 + (cilindraje * 0.02);
	}
	
	// Pago anticipado
	if (mes_pago >= 1 && mes_pago <= 3) {
		valor_total -= 5.0;
	}
	
	// Vehículo ecológico
	if (es_ecologico) {
		valor_total -= 10.0;
	}
	
	// Mes no correspondiente según placa
	int mes_asignado = (ultimo_digito_placa % 10) + 1;
	if (mes_pago > mes_asignado) {
		valor_total += 25.0;
	}
	
	// Multas
	valor_total += multas_transito;
	
	// Provincias con recargo
	if (strcmp(provincia, "Quito") == 0 || strcmp(provincia, "Guayaquil") == 0) {
		valor_total += 50.0;
	}
	
	// Revisión obligatoria
	if (revision_obligatoria && !es_ecologico) {
		valor_total += 30.0;
	}
	
	// Tasas fijas
	valor_total += 1.50; // tasa solidaria
	valor_total += 5.00; // tasa administrativa
	
	// Valor mínimo
	if (valor_total < 0.0) {
		valor_total = 0.0;
	}
	
	return valor_total;
}
	void registrarRevisiones(int revisiones[NUM_REVISIONES]) {
	printf("\n--- REGISTRO DE REVISIONES TECNICAS ---\n");
	for (int i = 0; i < NUM_REVISIONES; i++) {
		do {
			printf("¿Revision %d cumplida? (1 = Sí, 0 = No): ", i + 1);
			scanf("%d", &revisiones[i]);
		if (revisiones[i] != 0 && revisiones[i] != 1) {
		printf(" Valor invalido. Ingrese 1 o 0.\n");
}
             }
		while (revisiones[i] != 0 && revisiones[i] != 1);
}
		}
void mostrarEstadoRevisiones(Vehiculo v) {
	printf("\n--- ESTADO DE REVISIONES ---\n");
	int cumplidas = 0;
		for (int i = 0; i < NUM_REVISIONES; i++) {
		printf("Revision %d: %s\n", i + 1, v.revisiones[i] ? "Cumplida" : "No cumplida");
		if (v.revisiones[i]) {
	    cumplidas++;
	}
}
	if (cumplidas == NUM_REVISIONES) {
	printf("? Todas las revisiones estan cumplidas.\n");
	} else {
	printf("?? Faltan %d revisiones por cumplir.\n", NUM_REVISIONES - cumplidas);
	}
}





#ifndef REVISION_VEHICULAR_H
#define REVISION_VEHICULAR_H

#define MAX_PLACA 10
#define MAX_TIPO 20
#define MAX_AVALUO 20
#define MAX_ANIO 5
#define MAX_CEDULA 11
#define NUM_REVISIONES 3

typedef struct {
	char placa[MAX_PLACA];
	char cedula[MAX_CEDULA];
	char anio[MAX_ANIO];
	char tipo[MAX_TIPO];
	char avaluo[MAX_AVALUO];
	int revisiones[NUM_REVISIONES];
} Vehiculo;

// Prototipos de funciones
void validarMatricula(Vehiculo *v);

float calcular_matricula_vehicular(const char tipo_vehiculo[], int cilindraje, int anio_fabricacion,
	const char provincia[], float es_ecologico, float revision_obligatoria,
	float multas_transito, int mes_pago, int ultimo_digito_placa);

void registrarRevisiones(int revisiones[NUM_REVISIONES]);

void mostrarEstadoRevisiones(Vehiculo v);

#endif // REVISION_VEHICULAR_H
