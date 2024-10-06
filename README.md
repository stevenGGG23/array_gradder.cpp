
#include <iostream>
#include <fstream>
#include <iomanip>
#include <array>

// Function prototypes
std::array<int, 30> ProcessFile(std::ifstream &file, int &numGrades);
double CalculateFinalGrade(const std::array<int, 30> &grades, int numGrades);
int CalculateTotal(const std::array<int, 30> &grades, int numGrades);
char CalculateLetter(double finalGrade);

int main() {
    std::string filename;
    std::cout << "Enter the input file: ";
    std::cin >> filename;

    // Open the file
    std::ifstream file(filename);
    if (!file.is_open()) {
        std::cout << filename << " does not exist." << std::endl;
        return 1; // Exit if file doesn't exist
    }

    int numGrades = 0;
    std::array<int, 30> grades = ProcessFile(file, numGrades);
    file.close(); // Close file after reading

    int totalPoints = CalculateTotal(grades, numGrades);
    double finalGrade = CalculateFinalGrade(grades, numGrades);
    char letterGrade = CalculateLetter(finalGrade);

    // Output results
    std::cout << std::endl;
    std::cout << "Number of Grades:       " << std::setw(5) << numGrades << std::endl;
    std::cout << "Total Points Earned:    " << std::setw(5) << totalPoints << std::endl;
    std::cout << "Max Possible Points:    " << std::setw(5) << numGrades * 100 << std::endl;
    std::cout << std::endl;
    std::cout << "Final Grade: " << std::setw(7) << letterGrade << "     " << std::fixed << std::setprecision(1) << finalGrade << "%" << std::endl;
    return 0;
}

// Function to process the file and store grades in the array
std::array<int, 30> ProcessFile(std::ifstream &file, int &numGrades) {
    std::array<int, 30> grades = {0};  // Initialize array to zeros
    int grade;
    numGrades = 0;
    while (file >> grade && numGrades < 30) {
        grades.at(numGrades) = grade;
        numGrades++;
    }
    return grades;
}

// Function to calculate the total points
int CalculateTotal(const std::array<int, 30> &grades, int numGrades) {
    int total = 0;
    for (int i = 0; i < numGrades; ++i) {
        total += grades.at(i);
    }
    return total;
}

// Function to calculate the final grade percentage
double CalculateFinalGrade(const std::array<int, 30> &grades, int numGrades) {
    if (numGrades == 0) return 0.0;
    int totalPoints = CalculateTotal(grades, numGrades);
    int maxPoints = numGrades * 100;
    return (static_cast<double>(totalPoints) / maxPoints) * 100;
}

// Function to calculate the letter grade based on the final grade percentage
char CalculateLetter(double finalGrade) {
    if (finalGrade >= 90.0) {
        return 'A';
    } else if (finalGrade >= 80.0) {
        return 'B';
    } else if (finalGrade >= 70.0) {
        return 'C';
    } else if (finalGrade >= 60.0) {
        return 'D';
    } else {
        return 'F';
    }
}
