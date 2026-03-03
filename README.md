# practic
Практика 2026

Задание:
Написать одну из трёх сортировок статического массива (сортировку вставками, пузырьком, слиянием)

```go
package main

import (
    "fmt"
    "math/rand"
    "time"
)

func main() {
    //Генератор случайных чисел
    rand.Seed(time.Now().UnixNano())
    
    // Создание случайного массива
    var arr [100]int
    for i := 0; i < 100; i++ {
        // Генерация случайных чисел от -1000 до 1000
        arr[i] = rand.Intn(2001) - 1000
    }
    
    // Вывод исходного массива
    fmt.Println("Исходный массив:")
    for i := 0; i < 100; i++ {
        fmt.Printf("%d ", arr[i])
    }
    fmt.Println()

    // Пузырьковая сортировка
    for i := 0; i < len(arr)-1; i++ {
        for j := 0; j < len(arr)-i-1; j++ {
            if arr[j] > arr[j+1] {
                arr[j], arr[j+1] = arr[j+1], arr[j]
            }
        }
    }
    
    // Вывод отсортированного массива
    fmt.Println("Отсортированный массив:")
    for i := 0; i < 100; i++ {
        fmt.Printf("%d ", arr[i])
    }
}
