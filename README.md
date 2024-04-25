# PDF-to-Word-Converter-using-C++
#include <iostream>
#include <cstring>
#include <cstdlib>
#include <string>

using namespace std;

int main() {
    string find_command = "find /home/swayam/Documents -type f -name '*.pdf' | shuf -n 1";
    
    FILE* find_pipe = popen(find_command.c_str(), "r");
    if (!find_pipe) {
        cerr << "Error executing find command." << endl;
        return 1;
    }
    char path[1024];
    fgets(path, sizeof(path), find_pipe);
    pclose(find_pipe);
    
    path[strlen(path) - 1] = '\0';
    
    string pdftotext_command = "pdftotext '" + string(path) + "' temp.txt";
    int pdftotext_result = system(pdftotext_command.c_str());
    if (pdftotext_result != 0) {
        cerr << "Error converting PDF to text." << endl;
        return 1;
    }
    
    string libreoffice_command = "libreoffice --headless --convert-to docx --outdir /home/swayam/Videos temp.txt";
    int libreoffice_result = system(libreoffice_command.c_str());
    if (libreoffice_result != 0) {
        cerr << "Error converting text to Word (docx)." << endl;
        return 1;
    }
    
    string remove_command = "rm temp.txt";
    int remove_result = system(remove_command.c_str());
    if (remove_result != 0) {
        cerr << "Error removing temporary text file." << endl;
        return 1;
    }
    
    cout << "Conversion successful." << endl;
    
    return 0;
}
