#include <iostream>
#include <string>
#include <vector>
#include <unordered_map>

class User {
public:
    std::string username;
    std::string password;
    User(std::string uname, std::string pwd) : username(uname), password(pwd) {}
    virtual void display() = 0; // Pure virtual function
};

class JobSeeker : public User {
public:
    std::string resume;
    JobSeeker(std::string uname, std::string pwd) : User(uname, pwd) {}
    void uploadResume(std::string resumeText) {
        resume = resumeText;
    }
    void display() override {
        std::cout << "Job Seeker: " << username << std::endl;
    }
};

class Employer : public User {
public:
    Employer(std::string uname, std::string pwd) : User(uname, pwd) {}
    void display() override {
        std::cout << "Employer: " << username << std::endl;
    }
};
class Job {
public:
    std::string title;
    std::string description;
    std::string company;
    Job(std::string t, std::string d, std::string c) : title(t), description(d), company(c) {}
    void display() {
        std::cout << "Job Title: " << title << std::endl;
        std::cout << "Description: " << description << std::endl;
        std::cout << "Company: " << company << std::endl;
    }
};
std::unordered_map<std::string, JobSeeker> jobSeekers;
std::unordered_map<std::string, Employer> employers;
std::vector<Job> jobs;

void registerJobSeeker(std::string uname, std::string pwd) {
    jobSeekers[uname] = JobSeeker(uname, pwd);
}

void registerEmployer(std::string uname, std::string pwd) {
    employers[uname] = Employer(uname, pwd);
}

void postJob(std::string title, std::string description, std::string company) {
    jobs.push_back(Job(title, description, company));
}
int main() {
    int choice;
    std::string username, password, title, description, company, resumeText;

    while (true) {
        std::cout << "1. Register Job Seeker\n2. Register Employer\n3. Post Job\n4. Upload Resume\n5. Display Jobs\n6. Exit\n";
        std::cin >> choice;

        switch (choice) {
        case 1:
            std::cout << "Enter username: ";
            std::cin >> username;
            std::cout << "Enter password: ";
            std::cin >> password;
            registerJobSeeker(username, password);
            break;
        case 2:
            std::cout << "Enter username: ";
            std::cin >> username;
            std::cout << "Enter password: ";
            std::cin >> password;
            registerEmployer(username, password);
            break;
        case 3:
            std::cout << "Enter job title: ";
            std::cin >> title;
            std::cout << "Enter job description: ";
            std::cin >> description;
            std::cout << "Enter company name: ";
            std::cin >> company;
            postJob(title, description, company);
            break;
        case 4:
            std::cout << "Enter username: ";
            std::cin >> username;
            if (jobSeekers.find(username) != jobSeekers.end()) {
                std::cout << "Enter resume text: ";
                std::cin.ignore();
                std::getline(std::cin, resumeText);
                jobSeekers[username].uploadResume(resumeText);
            } else {
                std::cout << "Job seeker not found.\n";
            }
            break;
        case 5:
            for (const auto& job : jobs) {
                job.display();
                std::cout << "----------------\n";
            }
            break;
        case 6:
            return 0;
        default:
            std::cout << "Invalid choice. Try again.\n";
        }
    }
    return 0;
}
