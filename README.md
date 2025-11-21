# Proyecto Final Laboratorio Kathará

## tercer commit

- **Implementación de enrutamiento OSPF:**  
  Se configuró OSPF en todos los routers de la topología, respetando áreas, adyacencias y métricas.

- **Configuración de áreas:**  
  Se establecieron correctamente las áreas OSPF.
    Area 0: RA →  RB → RC → RZY
    Area 1: RH →  RG → RF → RE
    Area 2: RWX → RJ → RK → RL
 
    Interconexiones entre zonas a través de RA–RH–RWX, RB–RG–RK, RC–RF–RK, RZY–RE–R 
    en carpetas de imagenes se encuentr la tabla de costos

- **Asignación de costos:**  
  Se creó un archivo `metricas.txt` con todos los costos asignados por interfaz.

- **Generación y verificación del árbol SPF:**  
  Se calculó el árbol SPF desde el router **RA** (router con salida a Internet).

- **Propagación de rutas y pruebas de convergencia:**  
  Se realizaron reinicios de routers y modificaciones de métricas para validar el recálculo automático de rutas.

---

## ▶️ Comandos utilizados para levantar o reiniciar el laboratorio

```bash
kathara lstart --noterminals
kathara lrestart --noterminals ```

---
## Carpeta de imágenes
Todas las imágenes del proyecto están en la carpeta `imagenes`.

### Topología de red

![Topología](imágenes/topologia_tp_final_1.jpg)
