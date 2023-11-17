#include <functional>
#include <iostream>
#include <string>
using namespace std;

// Use the "interpret" function to convert your programme.

// This interprets lambda expressions.
string functionate(string program) {
    string out;
    // This declares a flag for global lambdas, to prevent capture errors.
    bool global = false;
    // The meat of the function loops through your programme, making changes where neccessary.
    for(int i = 0; i < program.size(); i++) {
        // Check for the "global" keyword.
        if(program.substr(i, 6) == "global") {
            global = true;
            i += 6;
        }
        // Check for lambdas, rewriting them in <functional> C++ syntax.
        if(program.substr(i, 6) == "lambda" && program.substr(i, 7) != "lambda<") {
            out += global ? "[]" : "[=]";
            global = false;
            i += 6;
            for(i; program[i] != '.'; i++) {
                out += program[i];
            }
            out += " {\n\treturn ";
            for(i++; program[i] != ';'; i++) {
                out += program[i];
            }
            out += ";\n};\n";
        }
        // Check for lambda type variablesm rewriting them as "auto" types.
        else if(program.substr(i, 5) == "func ") {
            out += "auto ";
            i += 4;
        }
        // Check for lambda type declarations, rewriting them as "function<>" types.
        else if (program.substr(i, 7) == "lambda<") {
            i += 7;
            out += "function<";
            for(i; program.substr(i, 4) != " of "; i++) {
                out += program[i];
            }
            i += 4;
            out += "(";
            for(i; program[i] != '>'; i++) {
                out += program[i];
            }
            out += ")>";
        }
        // If a functional feature isn't being used, simply copy the code character-for-character.
        else {
            out.push_back(program[i]);
        }
    }
    return out;
}

string interpret(string program) {
    string previous = program;
    program = functionate(program);
    while(program != previous) {
        previous = program;
        program = functionate(program);
    }
    return previous;
}

int main() {
    cout << interpret("#include <functional>\n#include <iostream>\nusing namespace std;\n\nfunc add = global lambda (float a).lambda(float b).a + b;\n\nint main() {\n\tcout << add(2)(2);\n}");
}
