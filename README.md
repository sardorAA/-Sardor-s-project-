# -Sardor-s-project-
calculator
#include <iostream>
#include <iomanip>
#include <sstream>
#include <string>
#include <cmath>
using namespace std;

// Функция вычислений
void calculate(double x) {
    try {
        double y1 = tan(0.1 * M_PI * x * x) + x;
        double y2 = exp(x + 1) - sin(x + M_PI);
        double y3 = pow(pow(sin(x), 2) + 2, 1.0 / 3.0);

        cout << fixed << setprecision(4);
        cout << "\nДля x = " << x << ":\n";
        cout << "  y1 = " << y1 << endl;
        cout << "  y2 = " << y2 << endl;
        cout << "  y3 = " << y3 << endl;
    } catch (...) {
        cout << "❌ Ошибка вычислений" << endl;
    }
}

int main() {
    cout << "📌 Математический калькулятор (C++)" << endl;
    cout << "Введите одно или несколько значений x через пробел." << endl;
    cout << "Для выхода введите: exit" << endl;

    string input;
    while (true) {
        cout << "\nВведите x: ";
        getline(cin, input);

        if (input == "exit") {
            cout << "✅ Программа завершена." << endl;
            break;
        }

        stringstream ss(input);
        double x;
        while (ss >> x) {
            calculate(x);
        }

        if (ss.fail() && !ss.eof()) {
            cout << "❌ Ошибка: найдено некорректное значение." << endl;
        }
    }

    return 0;
}
