#include <iostream>
#include <vector>
#include <queue>
#include <thread> 
#include <functional> 
#include <mutex>
using namespace std;
mutex stop;
void bfs(vector <vector<int>> &graf, int start) {
	int liczba_wezlow = 0;
	queue<int> kolejka;
	liczba_wezlow = graf.size();
	vector<bool> byl_odwiedzony;
	for (int j = 0; j < liczba_wezlow; j++)
	{
		byl_odwiedzony.push_back(0);
	}

	kolejka.push(start);
	byl_odwiedzony[start] = 1;
	stop.lock();
	for (; !kolejka.empty();)
	{
		int teraz;
		int dzieci;
		teraz = kolejka.front();
		dzieci = graf[teraz].size();
		cout << kolejka.front()<<' ';
		kolejka.pop();


		for (int ile = 0; ile < dzieci; ile++)
		{
			if (byl_odwiedzony[graf[teraz][ile]] == 0) {
				kolejka.push(graf[teraz][ile]);
				byl_odwiedzony[graf[teraz][ile]] = 1;
			}
		}

		byl_odwiedzony[teraz] = 1;
	}
	cout << endl;
	stop.unlock();
}

int main()
{
	vector <vector<int>> graf, graf1;
	graf = { {1},{0,2,4},{1,3},{2,4},{1,3,5},{4,5,6,7},{5},{5} };
	graf1 = { {3,4},{4},{3,4},{0,2,5},{0,1,2},{3,6},{5} };
	int start1 = 0,start2=0;
	cout << "podaj wierzcholek startowy dla 1 grafu";
	cin >> start1;
	cout << "podaj wierzcholek startowy dla 2 grafu";
	cin >> start2;
	
	thread watek1(bfs, ref(graf), start1);
	thread watek2(bfs, ref(graf1), start2);

	
	watek1.join();
	watek2.join();

}
