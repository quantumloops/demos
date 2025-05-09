#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

class Job {
public:
    int id, deadline, profit;

    Job(int id, int deadline, int profit) {
        this->id = id;
        this->deadline = deadline;
        this->profit = profit;
    }
};

class JobScheduler {
public:
    static bool compare(Job a, Job b) {
        return a.profit > b.profit;
    }

    void scheduleJobs(vector<Job>& jobs) {
        // Sort jobs by profit
        sort(jobs.begin(), jobs.end(), compare);

        // Find maximum deadline
        int maxDeadline = 0;
        for (auto job : jobs) {
            maxDeadline = max(maxDeadline, job.deadline);
        }

        // Slots for scheduling
        vector<int> slot(maxDeadline + 1, -1);

        int countJobs = 0;
        int totalProfit = 0;

        for (auto job : jobs) {
            // Find a free slot from job.deadline to 1
            for (int j = job.deadline; j > 0; j--) {
                if (slot[j] == -1) {
                    slot[j] = job.id;
                    countJobs++;
                    totalProfit += job.profit;
                    break;
                }
            }
        }

        cout << "Total Jobs Done: " << countJobs << endl;
        cout << "Total Profit: " << totalProfit << endl;
    }
};

int main() {
    vector<Job> jobs;
    jobs.push_back(Job(1, 2, 50));
    jobs.push_back(Job(2, 1, 15));
    jobs.push_back(Job(3, 2 , 10));
    jobs.push_back(Job(4, 1, 25));

    JobScheduler scheduler;
    scheduler.scheduleJobs(jobs);

    return 0;
}
