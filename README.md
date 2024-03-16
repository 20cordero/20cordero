import numpy as np
import matplotlib.pyplot as plt

# Solicitar valores al usuario
L = float(input("Ingrese la longitud del alimento (m): "))  
D = float(input("Ingrese el coeficiente de difusión (m²/s): "))  
t_final = float(input("Ingrese el tiempo de difusión (s): "))  

# Discretización del dominio espacial y temporal
dx = 0.01  
dt = 0.001  
nx = int(L / dx) + 1  
nt = int(t_final / dt) + 1  

# Condiciones iniciales y de contorno
C_inicial = float(input("Ingrese la concentración inicial del ingrediente (kg/m³): "))
C_izquierda = float(input("Ingrese la concentración en el extremo izquierdo (kg/m³): "))
C_derecha = float(input("Ingrese la concentración en el extremo derecho (kg/m³): "))

# Inicialización de la solución
C = np.zeros((nt, nx))  
C[0, :] = C_inicial  
C[:, 0] = C_izquierda  
C[:, -1] = C_derecha  

# Cálculo de la solución utilizando diferencias finitas
r = D * dt / dx ** 2  
for n in range(nt - 1):
    for i in range(1, nx - 1):
        C[n + 1, i] = C[n, i] + r * (C[n, i + 1] - 2 * C[n, i] + C[n, i - 1])

# Creación de la malla espacial y temporal
x = np.linspace(0, L, nx)
t = np.linspace(0, t_final, nt)

# Graficar la evolución de la concentración
plt.figure(figsize=(10, 6))
for n in range(0, nt, int(nt/10)):  # Graficar cada 10% del tiempo total
    plt.plot(x, C[n, :], label=f"t = {t[n]:.2f} s", linewidth=2)
plt.xlabel("Posición (m)", fontsize=12)
plt.ylabel("Concentración (kg/m³)", fontsize=12)
plt.title("Evolución de la concentración del ingrediente", fontsize=14)
plt.legend(fontsize=10)
plt.grid(True)
plt.tight_layout()
plt.show()
