As refatorações sugeridas podem estar presentes no livro Refactoring, de Martin Fowler, ou talvez em algum outro lugar. Se este for o caso, manda um PR sugerindo que eu coloque as referências :). 



## Usar mais de um dado do mesmo objeto em uma função fora do objeto

### Código problemático

```
boolean valido = criaCampeonatoRequest.getNumeroTimes() == criaCampeonatoRequest.getTimes().size()
```

### Uma possível versão melhor do mesmo código

```
boolean valido = criaCampeonatoRequest.numeroTimesValido();
```

### Por que eu faria essa refatoração?


* usar o mesmo valor duas vezes em um fluxo
* operar sobre o dado do objeto
* quando sua collection merece uma abstração só para ela
* toda indireção precisa dividir carga cognitiva
* toda generalização aumenta a carga cognitiva, use com moderação. 
