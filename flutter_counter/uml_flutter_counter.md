# Bloc Library Tutorial - Flutter Counter

Estudo da biblioteca Flutter_Bloc.

Nota: Os diagramas podem ser visualizados com o uso da extensão _Markdown Preview Enhancer_ do Visual Studio Code. https://shd101wyy.github.io/markdown-preview-enhanced/ . O Markdown Preview Enhancer usa o PlantUML para renderizar os diagramas de estado https://plantuml.com/ e o js-sequence-diagrams para renderizar os diagramas de sequência https://bramp.github.io/js-sequence-diagrams/ .

Estudos dos tutoriais que fazem parte do repositório do Bloc em https://github.com/felangel/bloc.

### DIAGRAMA DE ATIVIDADE

```plantuml

start

partition CounterApp {
  :Criar widget CounterPage;
}

partition CounterPage {
  partition "build(context)" {
    partition BlocProvider {
      :Criar CounterCubit;
      note right
        state = 0
      end note
      :Criar CounterView widget;
    }
  }
}

partition CounterView {

  partition "build(context)" {
    split
      :Exibir AppBar;
        partition "BlocBuilder<CounterCubit, int>" {
          (A)
          if ("Estado emitido?") then (true)
              :Exibir texto **$state**;
          else
            (A)
            detach
          endif
          (A)
          detach
        }
    split again
      partition "FloatingActionButton" {
        :Exibir botão **+**;
        :Exibir botão **-**;
        (B)
        if ("Botão + pressed?") then (true)
          #lightblue:(I)
        endif
        if ("Botão - pressed?") then (true)
          #lightblue:(D)
        endif
        (B)
        detach
      }
    end split
  }
}
detach

partition CounterCubit {
  split
    -[hidden]->
    #lightblue:(I)
    #salmon:emit (state + 1);
  split again
    -[hidden]->
    #lightblue:(D)
    #salmon:emit (state - 1);
  end split
    (B)
}
```

### DIAGRAMA DE CLASSE

```plantuml

package Cubit {
  class CounterCubit <<Cubit>> {
    +increment()
    +decrement()
  }
}

package "User Interface" {
  class CounterView <<StatelessWidget>>
  class CounterPage <<StatelessWidget>>
  CounterView --* CounterPage
}

package main() {
  class CounterApp << MaterialApp >> {

  }
  class CounterObserver << BlockObserver >> {
    -onChange()
  }
  class BlocOverrides {
    runZoned(CounterApp(), CounterObserver())
  }
  CounterApp -- BlocOverrides
  CounterObserver -- BlocOverrides
}

CounterApp *-- CounterPage
CounterPage *-- CounterCubit
CounterView o-- CounterCubit


```
