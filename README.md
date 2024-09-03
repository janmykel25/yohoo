#include <iostream>
#include <direct.h> 
#include <windows.h>  

using namespace std;

void listFiles();
void createDirectory();
void changeDirectory();

int main() {
    int choice;
    do {
        cout << "Directory Management System\n";
        cout << "1. List of files\n";
        cout << "2. Create a new directory\n";
        cout << "3. Change the working directory\n";
        cout << "4. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                listFiles();
                break;
            case 2:
                createDirectory();
                break;
            case 3:
                changeDirectory();
                break;
            case 4:
                cout << "Exiting the program.\n";
                break;
            default:
                cout << "Invalid choice. Please try again.\n";
        }
    } while (choice != 4);

    return 0;
}

void listFiles() {
    WIN32_FIND_DATA findFileData;
    HANDLE hFind = FindFirstFile("*.*", &findFileData);

    if (hFind == INVALID_HANDLE_VALUE) {
        cout << "Error: Unable to access directory.\n";
        return;
    } 

    do {
        // Skip current (.) and parent (..) directory entries
        if (strcmp(findFileData.cFileName, ".") != 0 && strcmp(findFileData.cFileName, "..") != 0) {
            cout << findFileData.cFileName << endl;
        }
    } while (FindNextFile(hFind, &findFileData) != 0);

    FindClose(hFind);
}

void createDirectory() {
    char dirname[100];
    cout << "Enter directory name: ";
    cin >> dirname;
    if (_mkdir(dirname) == 0) {
        cout << "Directory created successfully.\n";
    } else {
        cout << "Error: Unable to create directory. It may already exist.\n";
    }
}

void changeDirectory() {
    int choice;
    char dirname[100];
    cout << "1. Move one step back (to the parent directory)\n";
    cout << "2. Move to the root directory\n";
    cout << "3. Move to a specific directory\n";
    cout << "Enter your choice: ";
    cin >> choice;

    switch (choice) {
        case 1:
            if (_chdir("..") == 0) {
                cout << "Moved to the parent directory.\n";
            } else {
                cout << "Error: Unable to move to the parent directory.\n";
            }
            break;
        case 2:
            if (_chdir("\\") == 0) {
                cout << "Moved to the root directory.\n";
            } else {
                cout << "Error: Unable to move to the root directory.\n";
            }
            break;
        case 3:
            cout << "Enter directory name: ";
            cin >> dirname;
            if (_chdir(dirname) == 0) {
                cout << "Changed to the specified directory.\n";
            } else {
                cout << "Error: Unable to change to the specified directory.\n";
            }
            break;
        default:
            cout << "Invalid choice.\n";
    }
}
