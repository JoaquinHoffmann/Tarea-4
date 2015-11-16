# Tarea-4

import numpy as np

nx = 5
P = np.zeros((nx,1))
U = np.zeros((nx-1,1)) #Velocidad en los nodos de velocidad
A = np.zeros((nx,1))
dx = 2.0/(nx-1)

#Calculo de areas
A[0,0] = 0.5
A[nx-1,0] = 0.1
def calc_area(A0,An,l):
    A= 2*((-(A0 - An)/4)*l + (A0/2))
    return A

for i in range(1,nx-1):
    A[i,0]=calc_area(A[0,0],A[nx-1,0],i*dx)
    
#Adivinar velocidades y presiones:
P[0,0] = 10.0
P[nx-1,0] = 0.0 
rho = 1.0
vel_caras = np.zeros((nx,1)) #velocidad en los nodos de presion (caras) estimada
vel_caras[0,0] = np.sqrt((P[0,0]-P[nx-1,0])*2.0/(rho*(1-(A[nx-1,0]/A[0,0])**2)))/(A[0,0]/A[nx-1,0]) #Son las velocidades de las caras

for i  in range(1,nx):
    vel_caras[i,0] = vel_caras[i-1,0]*A[i-1,0]/(A[i,0])
    P[i,0] = P[i-1,0] - 0.5*rho*((vel_caras[i,0]**2) - (vel_caras[i-1,0]**2))
    print(vel_caras[i,0])
    
for i in range(0,nx-1):
    U[i,0] = (vel_caras[i+1,0]+vel_caras[i,0])/2

    

