# practic
Практика 2026

1 задание:
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
```

2 задание:
Написать программу которая хранит данные об офисных сотрудниках. Сделать операцию добавления и удаления
(максимальное число сотрудников на добавление - 512)

```go
package main

import (
	"fmt"
	"bufio"
	"os"
	"strings"
	"strconv"
)

type Employee struct {
	Name     string
	Age      int
	Position string
	Salary   int
}

const maxEmployees = 512

var commands = `
1 - Добавить нового сотрудника
2 - Удалить сотрудника
3 - Вывести список сотрудников
4 - Выйти из программы
`

func main() {
	empls := [maxEmployees]*Employee{}
	scanner := bufio.NewScanner(os.Stdin)
	
	for {
		cmd := 0
		fmt.Print(commands)
		fmt.Print("Выберите действие: ")
		
		if !scanner.Scan() {
			continue
		}
		cmd, _ = strconv.Atoi(scanner.Text())

		switch cmd {
		case 1:
			// Поиск свободного места
			freeIndex := -1
			for i := 0; i < maxEmployees; i++ {
				if empls[i] == nil {
					freeIndex = i
					break
				}
			}
			
			if freeIndex == -1 {
				fmt.Println("Ошибка: достигнуто максимальное количество сотрудников (512)")
				continue
			}
			
			empl := new(Employee)
			fmt.Println("\n--- Добавление нового сотрудника ---")
			
			fmt.Print("Имя: ")
			if scanner.Scan() {
				empl.Name = strings.TrimSpace(scanner.Text())
			}
			
			fmt.Print("Возраст: ")
			if scanner.Scan() {
				empl.Age, _ = strconv.Atoi(scanner.Text())
			}
			
			fmt.Print("Позиция: ")
			if scanner.Scan() {
				empl.Position = strings.TrimSpace(scanner.Text())
			}
			
			fmt.Print("Зарплата: ")
			if scanner.Scan() {
				empl.Salary, _ = strconv.Atoi(scanner.Text())
			}
			
			empls[freeIndex] = empl
			fmt.Printf("Сотрудник %s успешно добавлен (ID: %d)\n", empl.Name, freeIndex+1)
			
		case 2:
			if !hasEmployees(empls) {
				fmt.Println("Список сотрудников пуст")
				continue
			}
			
			fmt.Println("\n--- Удаление сотрудника ---")
			fmt.Print("Введите ID сотрудника для удаления: ")
			
			if !scanner.Scan() {
				continue
			}
			id, _ := strconv.Atoi(scanner.Text())
			
			index := id - 1
			
			if index < 0 || index >= maxEmployees {
				fmt.Println("Ошибка: неверный ID")
				continue
			}
			
			if empls[index] == nil {
				fmt.Printf("Сотрудник с ID %d не найден\n", id)
				continue
			}
			
			name := empls[index].Name
			empls[index] = nil
			fmt.Printf("Сотрудник %s (ID: %d) удален\n", name, id)
			
		case 3:
			fmt.Println("\n--- Список всех сотрудников ---")
			if !hasEmployees(empls) {
				fmt.Println("Список сотрудников пуст")
				continue
			}
			
			fmt.Printf("%-5s %-15s %-8s %-20s %-10s\n", "ID", "Имя", "Возраст", "Позиция", "Зарплата")
			fmt.Println("------------------------------------------------------------")
			
			count := 0
			for i := 0; i < maxEmployees; i++ {
				if empls[i] != nil {
					fmt.Printf("%-5d %-15s %-8d %-20s %-10d\n", 
						i+1, empls[i].Name, empls[i].Age, empls[i].Position, empls[i].Salary)
					count++
				}
			}
			fmt.Println("------------------------------------------------------------")
			fmt.Printf("Всего сотрудников: %d/%d\n", count, maxEmployees)
			
		case 4:
			fmt.Println("Программа завершена")
			return
			
		default:
			fmt.Println("Неверный выбор. Пожалуйста, выберите 1-4")
		}
	}
}

func hasEmployees(empls [maxEmployees]*Employee) bool {
	for i := 0; i < maxEmployees; i++ {
		if empls[i] != nil {
			return true
		}
	}
	return false
}
```
