## **Последовательность действий выполняемых Java**

* JRE запускает Classloader'ы для загрузки классов в JVM
  * Bootstrap class loader - загружает основные библиотеки Java `<JAVA_HOME>/jre/lib`
  * Extensions class loader - загружает код в каталоги расширений `<JAVA_HOME>/jre/lib/ext`, либо любой другой каталог указанный `java.ext.dirs`
  * System class loader - загружает код найденный в `java.class.path`
  
  *Происходит загрузка, связывание, инициализация*
  
* JVM выделяет Run-Time Data Areas (области памяти), где содержатся Heap/Metaspace/Stacks

## Выполнение кода

* Class loader подгружает 'class JvmComprehension` в Metaspace
* Создается поток в Stacks
* В данном потоке создается frame для метода `public static void main(String[] args)`

### 1
* В `main()` frame записывается целочисленный примитив `int i = 1`

### 2
* В Heap создается объект `new Object()`, так как это объект из стандартной библиотеки java, то подгружать его не надо
* В `main()` frame записывается ссылка на этот объект `Object o`

### 3
* В Heap создается объект `Integer ii`
* Создается целочисленный примитив и происходит автоупаковка его в Integer `Integer ii = 2`, в `main()` frame записывается ссылка на этот объект

### 4
* frame `main()` отправляет сообщение `printAll()` и передает ему 3 аргумента: 1)ссылку на объект в Heap `Object o`, 2)примитив `int i`, 3)ссылку на объект в Heap `Integer ii` необходимые для его выполнения
* Создается новый frame для `printAll()` метода и он становится текущим

### 5
* В Heap создается объект `Integer uselessVar`
* Создается целочисленный примитив и происходит автоупаковка его в Integer `Integer uselessVar = 700`, в `printAll()` frame записывается ссылка на этот объект

### 6
* frame `printAll()` отправляет сообщение методу стандартной библиотеки `System.out.println` и передает ему ссылку на объект в Heap `String`:

  ## Формирование String для аргумента ##
  1 frame `printAll()` отправляет сообщение объекту в Heap `Object o` и его методу `toString()` без аргументов
    * Создается новый frame для `toString()` и становится текущим
    * frame `toString()` берет данные своих полей и возвращает их frame'у `printAll()` в виде ссылки на String, предварительно создав ее в Heap
    * frame `toString()` закрывается
  
  2 Создается `String` объект в Heap, примитив `int i = 1`(переданный в аргумент текущему frame) оборачивается в него
  
  3 Создается `String` объект в Heap, и в него оборачивается (предварительно развернутый из `Integer ii`) целочисленный примитив 
  
  * Создается новый объект String в Heap и в него добавляются эти 3 элемента, ссылка на этот объект и будет передана в аргумент
  
* Создается frame для `System.out.println` и становится текущим
* Производится печать в консоль текста из переданной ссылки на Heap
* frame `System.out.println` закрывается

* frame `printAll()` закрывается

### 7
* Создается объект `String` в Heap и в него помещается значение "finished"
* frame `main()` посылает сообщение методу `System.out.println` и передает ему ссылку на объект `String`["finished"]
* Создается новый frame `System.out.println` и становится текущим 
* frame `System.out.println` распечатывает текст из переданной ему ссылки в консоль 
* frame `System.out.println` закрывается

* frame `main()` закрывается

* Stack закрывается
