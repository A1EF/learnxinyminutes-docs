---
filename: learnparen-ru.paren
contributors:
  - ["KIM Taegyoon", "https://github.com/kimtg"]
translators:
  - ["Dmitry Bessonov", "https://github.com/TheDmitry"]
---

[Paren](https://bitbucket.org/ktg/paren) - это диалект языка Лисп. Он спроектирован как встроенный язык.

Примеры взяты <http://learnxinyminutes.com/docs/racket/>.

```scheme
;;; Комментарии
# комментарии

;; Однострочные комментарии начинаются с точки с запятой или символа решетки

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; 1. Примитивные типы данных и операторы
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

;;; Числа
123 ; int
3.14 ; double
6.02e+23 ; double
(int 3.14) ; => 3 : int
(double 123) ; => 123 : double

;; Обращение к функции записывается так: (f x y z ...),
;; где f - функция, а x, y, z, ... - операнды
;; Если вы хотите создать буквальный список данных, используйте (quote), чтобы
;; предотвратить ненужные вычисления
(quote (+ 1 2)) ; => (+ 1 2)
;; Итак, некоторые арифметические операции
(+ 1 1)  ; => 2
(- 8 1)  ; => 7
(* 10 2) ; => 20
(^ 2 3) ; => 8
(/ 5 2) ; => 2
(% 5 2) ; => 1
(/ 5.0 2) ; => 2.5

;;; Логический тип
true ; обозначает истину
false ; обозначает ложь
(! true) ; => false
(&& true false (prn "досюда не доходим")) ; => false
(|| false true (prn "досюда не доходим")) ; => true

;;; Символы - это числа (int).
(char-at "A" 0) ; => 65
(chr 65) ; => "A"

;;; Строки - это массив символов с фиксированной длиной.
"Привет, мир!"
"Benjamin \"Bugsy\" Siegel"   ; обратная косая черта экранирует символ
"Foo\tbar\r\n" ; включает управляющие символы в стиле Cи: \t \r \n

;; Строки тоже могут объединяться!
(strcat "Привет " "мир!") ; => "Привет мир!"

;; Строка может трактоваться подобно списку символов
(char-at "Apple" 0) ; => 65

;; Выводить информацию достаточно легко
(pr "Я" "Paren. ") (prn "Приятно познакомиться!")

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; 2. Переменные
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; Вы можете создать или инициализировать переменную, используя (set)
;; имя переменной может содержать любой символ, исключая: ();#"
(set some-var 5) ; => 5
some-var ; => 5

;; Обращение к переменной, прежде не определенной, вызовет исключение
; x ; => Неизвестная переменная: x : nil

;; Локальное связывание: Используйте лямбда-вычисление! `a' и `b' связывается
;; с `1' и `2' только в пределах (fn ...)
((fn (a b) (+ a b)) 1 2) ; => 3

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; 3. Коллекции
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

;;; Списки

;; Списки подобны динамическому массиву (vector). (произвольный доступ равен O(1).)
(cons 1 (cons 2 (cons 3 (list)))) ; => (1 2 3)
;; `list' - это удобный конструктор списков с переменным числом элементов
(list 1 2 3) ; => (1 2 3)
;; и quote может также использоваться для литеральных значений списка
(quote (+ 1 2)) ; => (+ 1 2)

;; Можно еще использовать `cons', чтобы добавить элемент в начало списка
(cons 0 (list 1 2 3)) ; => (0 1 2 3)

;; Списки являются основным типом, поэтому для них предусмотрено *много* функций
;; немного примеров из них:
(map inc (list 1 2 3))          ; => (2 3 4)
(filter (fn (x) (== 0 (% x 2))) (list 1 2 3 4))    ; => (2 4)
(length (list 1 2 3 4))     ; => 4

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; 3. Функции
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

;; Используйте `fn' для создания функций.
;; Функция всегда возвращает значение своего последнего выражения
(fn () "Привет Мир") ; => (fn () Привет Мир) : fn

;; Используйте скобки, чтобы вызвать все функции, в том числе лямбда-выражение
((fn () "Привет Мир")) ; => "Привет Мир"

;; Назначить функцию переменной
(set hello-world (fn () "Привет Мир"))
(hello-world) ; => "Привет Мир"

;; Вы можете сократить это, используя синтаксический сахар определения функции:
(defn hello-world2 () "Привет Мир")

;; Как и выше, () - это список аргументов для функции
(set hello
  (fn (name)
    (strcat "Привет " name)))
(hello "Стив") ; => "Привет Стив"

;; ... или, что эквивалентно, используйте синтаксический сахар определения:
(defn hello2 (name)
  (strcat "Привет " name))

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; 4. Равенство
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

;; для чисел используйте `=='
(== 3 3.0) ; => true
(== 2 1) ; => false

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; 5. Поток управления
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

;;; Условный оператор

(if true               ; проверка выражения
    "это - истина"   ; тогда это выражение
    "это - ложь") ; иначе другое выражение
; => "это - истина"

;;; Циклы

;; Цикл for для чисел
;; (for ИДЕНТИФИКАТОР НАЧАЛО КОНЕЦ ШАГ ВЫРАЖЕНИЕ ..)
(for i 0 10 2 (pr i "")) ; => печатает 0 2 4 6 8 10
(for i 0.0 10 2.5 (pr i "")) ; => печатает 0 2.5 5 7.5 10

;; Цикл while
((fn (i)
  (while (< i 10)
    (pr i)
    (++ i))) 0) ; => печатает 0123456789

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; 6. Изменение
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

;; Используйте `set', чтобы назначить новое значение переменной или памяти
(set n 5) ; => 5
(set n (inc n)) ; => 6
n ; => 6
(set a (list 1 2)) ; => (1 2)
(set (nth 0 a) 3) ; => 3
a ; => (3 2)

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; 7. Макросы
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

;; Макросы позволяют вам расширять синтаксис языка.
;; Paren-макросы легкие.
;; Фактически, (defn) - это макрос.
(defmacro setfn (name ...) (set name (fn ...)))
(defmacro defn (name ...) (def name (fn ...)))

;; Давайте добавим инфиксную нотацию
(defmacro infix (a op ...) (op a ...))
(infix 1 + 2 (infix 3 * 4)) ; => 15

;; Макросы приводят к неясному коду, т.е. вы можете затереть существующие переменные!
;; Они являются кодопреобразующей конструкцией.
```