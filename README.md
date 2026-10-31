# FDV-Tilemaps-2627

## Ejercicio 1

1. En esta primera tarea de la práctica, trabajaremos con los elementos fundamentales del mapa de juego, se debe trabajar con los objetos:
- Grid
- Tilemap
- Tile Palette

**Actividades a realizar:**
 
Partiendo de la escena implementada para la tarea de Sprites  construiremos un mapa de juego, que delimitará la zona en la que tendrá lugar la acción. Utiliza los recursos en el paquete generado para la práctica.Debes crear una paleta con los Tiles que consideres. Ajusta el tamaño de los Tiles al tamaño de celda del grid y subdivide aquellos Tiles que ocupen un grupo de celdas. Generar un mapa convencional que represente el entorno y límites de juego.

***Para este ejercicio decidí crear un mapa base con sprites para un juego Top-View, en el que se representa una casa con sus delimitaciones de paredes, suelo y jardin.***

![Ejercicio 1](./Media/Ejercicio1.png)

## Ejercicio 2

2. En este ejercicio se trabaja con el uso de varios Tilemaps en la misma escena, cada uno tendrá un orden de renderizado diferente, por lo que puede afectar a su visibilidad. Si te surgen problemas puedes actuar en el orden de la capa También es conveniente revisar el valor de Z, una ayuda puede ser la visualización 3D del proyecto.

**Actividades a realizar:**

- Crea dos Tilemaps adicionales de obstáculos. Uno puede representar elementos decorativos y otro obstáculos.
- Agrega a la capa de obstáculos la configuración necesaria para que el Tilemap se construya de forma independiente y el obstáculo actúe como tal. Esto puede dar problemas por diferentes causas:
    - Velocidad del renderizado muy alta: Activa detección de colisiones continua
    - Orden de las capas
    - Las capas permiten colisiones entre sí.
    - Tamaño y forma de los colliders. Se puede corregir editando el Sprite para adaptar la forma al objeto y aplicar Custom Physics Shape
    - Si has modificado la escala puede afectar a la detección de colisiones.
- Agrega una capa de objetos recolectables que hagan sumar puntos al jugador, y otra de objetos que hacen daño que le resten puntos.

***Tras agregar los Tilemaps, se configuraron las múltiples capas necesarias para distinguir la renderización de cada tipo de elemento en el juego. Se agregaron los colliders a los elementos que se consideraron obstáculos y se configuraron las capas de objetos recolectables y dañinos.***

***Una vez configuradas las capas y los colliders, se colocaron los tiles de distintos tipos para darle más vida al mapa, teniendo objetos tirados por el suelo, obstaculos dentro de la casa, y paredes que delimitan la zona de juego.***

![Ejercicio 2](./Media/Ejercicio2.png)

## Ejercicio 3

3. El último bloque de tareas con el que trabajaremos será implementar algunas mecánicas útiles. 

    Control de Movimiento 2D: En cada actualización, mediante el método Update(){ } avanzaremos el espacio que nos permite una velocidad speed, en el tiempo transcurrido. El intervalo de tiempo entre frames lo controla Time.deltaTime. Utiliza los ejes para dirigir la orientación del movimiento.
    
    Control del personaje 2D con rotación y avance hacia adelante: En cada actualización, mediante el método Update(){ } avanzaremos siempre hacia adelante el espacio que nos permite una velocidad speed, en el tiempo transcurrido. El eje X se usa para rotar y orientarnos hacia donde queremos caminar. El eje Y se usa para avanzar una vez que estamos orientados.

***Para este ejercicio se implementó un control de movimiento 2D con rotación y avance hacia adelante, en el que el jugador puede moverse por el mapa con las teclas WASD:***

```csharp
public class PlayerMovement : MonoBehaviour
{
    [SerializeField] private float speed = 3f;
    [SerializeField] private float rotationSpeed = 1100f;

    private void Update()
    {
        float horizontal = Input.GetAxis("Horizontal");
        float vertical = Input.GetAxis("Vertical");
        Vector3 movement = new Vector3(horizontal, vertical, 0f);

        if (movement.sqrMagnitude == 0f)
        {
            return;
        }

        movement.Normalize();
        transform.position += movement * speed * Time.deltaTime;

        float targetAngle = Mathf.Atan2(movement.y, movement.x) * Mathf.Rad2Deg;
        Quaternion targetRotation = Quaternion.Euler(0f, 0f, targetAngle);
        transform.rotation = Quaternion.RotateTowards(
            transform.rotation,
            targetRotation,
            rotationSpeed * Time.deltaTime);
    }
}
```

![Ejercicio 3](./Media/Ejercicio3.gif)
