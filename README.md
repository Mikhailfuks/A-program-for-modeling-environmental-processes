#include <iostream>
#include <vector>
#include <random>
#include <cmath>

using namespace std;

// Структура для представления популяции
struct Population {
    int size;
    double growthRate;
    double carryingCapacity;
};

// Функция для моделирования роста популяции
void updatePopulation(Population& population, double dt) {
    // Модель логистического роста
    double growth = population.growthRate * population.size * (1 - population.size / population.carryingCapacity);
    population.size += growth * dt;
}

// Структура для представления хищника
struct Predator {
    int size;
    double growthRate;
    double attackRate;
};

// Функция для моделирования популяции хищника
void updatePredator(Predator& predator, const Population& prey, double dt) {
    // Модель роста хищника зависит от численности жертвы
    double growth = predator.growthRate * predator.size * (1 - predator.size / (predator.attackRate * prey.size));
    predator.size += growth * dt;
}

// Функция для моделирования взаимодействия хищника и жертвы
void updateEcosystem(Population& prey, Predator& predator, double dt) {
    updatePopulation(prey, dt);
    updatePredator(predator, prey, dt);
}

int main() {
    // Начальные условия
    Population prey = {100, 0.1, 1000}; // Численность жертвы, скорость роста, емкость среды
    Predator predator = {20, 0.05, 0.01}; // Численность хищника, скорость роста, скорость атаки

    // Шаг моделирования
    double dt = 0.1;

    // Моделирование экосистемы
    for (int i = 0; i < 100; i++) {
        updateEcosystem(prey, predator, dt);
        cout << "Шаг " << i << ": Жертва: " << prey.size << ", Хищник: " << predator.size << endl;
    }

    return 0;
}
