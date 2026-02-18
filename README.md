#include <iostream>
#include <fstream>
#include <string>
#include <vector>
#include <iomanip>   // for setw, setprecision if needed
#include <ctime>     // optional: for current date/time
using namespace std;

class Student {
public:
    string indexNumber;   // e.g. "EE20001" or "2023001"
    string name;
    // You can add more: program, level, etc. if you want

    Student() {}  // default constructor

    Student(string idx, string n) : indexNumber(idx), name(n) {}

    void display() const {
        cout << left << setw(15) << indexNumber << setw(30) << name << endl;
    }
};

struct AttendanceRecord {
    string indexNumber;
    char status;   // 'P' = Present, 'A' = Absent, 'L' = Late
};

class AttendanceSession {
public:
    string courseCode;     // e.g. "EE201"
    string date;           // "2026-02-18"  (string is simplest)
    string startTime;      // "08:00"
    int durationHours;     // e.g. 2

    vector<AttendanceRecord> records;

    AttendanceSession() : durationHours(0) {}

    void addRecord(const string& idx, char stat) {
        records.push_back({idx, stat});
    }

    void displayAttendance(const vector<Student>& allStudents) const {
        cout << "\nAttendance for " << courseCode << " on " << date 
             << " (" << startTime << ", " << durationHours << " hrs)\n";
        cout << left << setw(15) << "Index" << setw(30) << "Name" 
             << setw(12) << "Status" << endl;
        cout << string(60, '-') << endl;

        int present = 0, absent = 0, late = 0;

        for (const auto& rec : records) {
            bool found = false;
            for (const auto& stu : allStudents) {
                if (stu.indexNumber == rec.indexNumber) {
                    cout << left << setw(15) << rec.indexNumber 
                         << setw(30) << stu.name 
                         << setw(12) << (rec.status == 'P' ? "Present" :
                                         rec.status == 'L' ? "Late" : "Absent") << endl;
                    found = true;

                    if (rec.status == 'P') present++;
                    else if (rec.status == 'L') late++;
                    else absent++;

                    break;
                }
            }
            if (!found) {
                cout << left << setw(15) << rec.indexNumber 
                     << setw(30) << "(Unknown student)" 
                     << setw(12) << (rec.status == 'P' ? "Present" : 
                                     rec.status == 'L' ? "Late" : "Absent") << endl;
            }
        }

        cout << "\nSummary:\n";
        cout << "Present: " << present << " | Late: " << late << " | Absent: " << absent << endl;
        cout << "Total marked: " << records.size() << endl;
    }

    string getFilename() const {
        return "session_" + courseCode + "_" + date + ".txt";
    }
};
