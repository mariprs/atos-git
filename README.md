# Documentação com Markdown

Escrever código `c++` com highlight

```cpp
#include<iostream>

int main() {
    std::cout << "exemplo" << std::endl;
    return 0;
}
```

Escrever `shellscript` dentro

```sh
# Buildar os containers
COMPOSE_DOCKER_CLI_BUILD=1 DOCKER_BUILDKIT=1 docker-compose build

# Rodar os containers
COMPOSE_PROJECT_NAME=getby docker-compose up
```

## LaTeX (Sem suporte para celular)

Equações podem ser escritas usando LaTeX:

$$V_{sphere} = \frac{4}{3}\pi r^3$$

$$\sum_{n=1}^{\infty} 2^{-n} = 1$$

$$\lim_{x\to\infty} f(x)$$

## Mermaid (Sem suporte para celular)

Para criar os seus flowcharts é possível usar o [editor online](https://mermaid.live/)

Exemplos de representação de infraestrutura usando `mermaid`

```mermaid
graph TD
    usuario[usuario] --> front[front_end]
    usuario[usuario] --> backend
    subgraph backend_network
    direction LR
    backend[backend] --> database[database]
    end
    style backend_network fill:#fff,stroke:#97f,stroke-width:2px,color:#000
```

Exemplo de documentação de código UML

```mermaid
classDiagram
    class State{
      +int Stay$
      +int Title$
      +int Settings$
      +int Game$
    }

    class Screen{
      +Clock fps
      +State state
      +Content content
      +change(State)
    }

    class Content{
      +update()*
      +draw()*
    }

    Content <|-- Game
    class Game{
      +Logic game
      +int border
      +int brick_size
      +Brick brick_skin
      +update()
      +draw()
      +draw_grid()
      +draw_border()
      +draw_stats()
      +draw_score()
      +draw_next_tetromino()
      +draw_swap_tetromino()
      +draw_current_tetromino()
      +draw_shadow_tetromino()
      +draw_tetromino()
    }

    Content <|-- Settings
    class Settings{
      +list[str] options
      +update()
      +draw()
    }

    Content <|-- Title
    class Title{
      +list[str] options
      +int border
      +update()
      +draw()
      +draw_buttons()
      +draw_title()
    }

    class Tetromino_generator{
      +dict[Shape, int] counter
      +Randomizer randomizer
      +list[Shape] history
      +next_tetromino()
      +add_history()
    }

    class Randomizer{
      +get_random()*
    }

    Randomizer <|-- Classic_tetris
    class Classic_tetris{
      +get_random()*
    }

    Randomizer <|-- Modern_tetris
    class Modern_tetris{
      +fill_tetromino_bag()*
      +get_random()*
    }

    Randomizer <|-- TGM
    class TGM{
      +dict[int, Shape] tetrominos
      +int tries
      +bool first
      +get_random()*
    }

    class Logic{
      +int lock_delay
      +int frames
      +int score
      +list[list[tuple[int,int,int]]] grid
      +Optional[Tetromino] current_tetromino
      +bool can_swap
      +Optional[Tetromino] hold_tetromino
      +list[Tetromino] next_tetrominos
      +Tetromino_generator generator
      +input_action()
      +next_tetromino()
      +swap_tetromino()
      +lock_tetromino()
      +clear_row()
      +check_alive()
    }

    class Colors{
      +int BLACK$
      +int GRAY$
      +int WHITE$
      +int RED$
      +int GREEN$
      +int BLUE$
      +int CYAN$
      +int YELLOW$
      +int PURPLE$
      +int ORANGE$
    }

    class Color_mod{
      +dict[Shape, Colors] get_color$
      +get_shadow_from_color()
      +get_shadow_from_tuple()
      +get_light_from_color()
      +get_light_from_tuple()
    }

    class Brick{
      +draw_brick()*
    }

    Brick <|-- Standard_Brick
    class Standard_Brick{
      +draw_brick()
    }

    Brick <|-- Line_Brick
    class Line_Brick{
      +draw_brick()
    }

    Brick <|-- Shiny_Brick
    class Shiny_Brick{
      +draw_brick()
    }

    Brick <|-- Open_Brick
    class Open_Brick{
      +draw_brick()
    }

    Brick <|-- Border_Brick
    class Border_Brick{
      +draw_brick()
    }

    class Shape{
      +list[list[str]] SHAPE_O$
      +list[list[str]] SHAPE_I$
      +list[list[str]] SHAPE_T$
      +list[list[str]] SHAPE_S$
      +list[list[str]] SHAPE_Z$
      +list[list[str]] SHAPE_J$
      +list[list[str]] SHAPE_L$
    }

    class SRS{
      +list[list[tuple[int, int]]] rot
    }

    class Tetromino{
      +get_shape()
      +move()
      +down()
      +hard_drop()
      +rotate_180()
      +rotate()
      +check()
      +reset()
      +reset_delay()
    }
```



