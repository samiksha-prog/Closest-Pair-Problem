# Closest-Pair-Problem
### Using Divide and Conquer Algorithm
Closest Pair Problem is to find the minimum distance between two points among n number of points. In order to find the distance between two points we used the distance formula. 
### Input
Enter space seperated X and Y coordinates for one point in a line. Hence enter 'n' points in 'n' lines.
#### Test case 1
6 3 <br /> 
3 2 <br /> 
8 5 <br /> 
3.5 5 <br /> 
1 1 <br /> 
4 4.5 
#### Test case 2
4 4 <br /> 
6 3 <br /> 
4 8 <br /> 
2 7 <br /> 
5 10 <br /> 
8 5 
#### Test case 3
4.01 3 <br /> 
3.9 3 <br /> 
8 5 <br /> 
3.5 5 <br /> 
1 1 <br /> 
4 4.5 <br /> 
9 12 <br /> 
24 16
### Code
```
#include <iostream>
#include <float.h>
#include <stdlib.h>
#include <math.h>
using namespace std;

void merge(double arr[][2], int l, int m, int r, int in)
{
    int i, j, k;
    int n1 = m - l + 1;
    int n2 = r - m;
    double L[n1][2], R[n2][2];
    for (i = 0; i < n1; i++){
        L[i][0] = arr[l + i][0];
        L[i][1] = arr[l + i][1];
    }
    for (j = 0; j < n2; j++){
        R[j][0] = arr[m + 1 + j][0];
        R[j][1] = arr[m + 1 + j][1];
    }
    i = 0;
    j = 0;
    k = l;
    while (i < n1 && j < n2) {
        if (L[i][in] <= R[j][in]) {
            arr[k][in] = L[i][in];
            arr[k][(in+1)%2] = L[i][(in+1)%2];
            i++;
        }
        else {
            arr[k][in] = R[j][in];
            arr[k][(in+1)%2] = R[j][(in+1)%2];
            j++;
        }
        k++;
    }
    while (i < n1) {
        arr[k][in] = L[i][in];
        arr[k][(in+1)%2] = L[i][(in+1)%2];
        i++;
        k++;
    }
    while (j < n2) {
        arr[k][in] = R[j][in];
        arr[k][(in+1)%2] = R[j][(in+1)%2];
        j++;
        k++;
    }
}
void mergeSort(double arr[][2], int l, int r, int in)
{
    if (l < r) {
        int m = l + (r - l) / 2;
        mergeSort(arr, l, m, in);
        mergeSort(arr, m + 1, r, in);
        merge(arr, l, m, r, in);
    }
}
float dist(double p1x, double p2x, double p1y, double p2y)
{
	return sqrt( pow(p1x - p2x,2) +pow(p1y - p2y,2));
}
float bruteForce(double P[][2], int n)
{
	float min = FLT_MAX;
	for (int i = 0; i < n; ++i)
		for (int j = i+1; j < n; ++j)
			if (dist(P[i][0], P[j][0],P[i][1],P[j][1]) < min)
				min = dist(P[i][0], P[j][0],P[i][1],P[j][1]);
	return min;
}
float min(float x, float y)
{
	return (x < y)? x : y;
}
float stripClosest(double strip[][2], int size, float d)
{
	float min = d;
	mergeSort(strip,0,size-1,1);
	for (int i = 0; i < size; ++i)
		for (int j = i+1; j < size && (strip[j][1] - strip[i][1]) < min; ++j)
			if (dist(strip[i][0],strip[j][0],strip[i][1],strip[j][1]) < min)
				min = dist(strip[i][0],strip[j][0],strip[i][1],strip[j][1]);
	return min;
}
float closestUtil(double P[][2], int n)
{
	if (n <= 3)
		return bruteForce(P, n);
	int mid = n/2;
	double midPoint[2] = {P[mid][0],P[mid][1]};
	float dl = closestUtil(P, mid);
	float dr = closestUtil(P + mid, n-mid);
	float d = min(dl, dr);
    double strip[n][2];
	int j = 0;
	for (int i = 0; i < n; i++)
		if (abs(P[i][0] - midPoint[0]) < d){
            strip[j][0] = P[i][0];
            strip[j][1] = P[i][1];
            j++;
		}
	return min(d, stripClosest(strip, j, d));
}
float closest(double P[][2], int n)
{
	mergeSort(P,0,n-1,0);
	return closestUtil(P, n);
}
int main()
{
    int t;
    cout << "Enter the number of test cases: ";
    cin >> t;
    for(int k=0;k<t;k++){
        int arr_size;
        cout << "Enter the number of points: ";
        cin >> arr_size;
        double points[arr_size][2];
        cout << "Enter the X and Y co-ordinates of the points: \n";
        for(int i=0;i<arr_size;i++)
                for(int j=0;j<2;j++)
                    cin >> points[i][j];
        cout << "The smallest distance between two points is " << closest(points, arr_size) << "\n";
    }
    return 0;
}
```
### Output
![Screenshot (17)](https://user-images.githubusercontent.com/83723880/191329333-1ec17368-9990-47fc-bc01-06488a4006f2.png)
### Explanation
1) Sort the points with respect to X coordinates. Merge sort is used to sort the points on the basis of X or Y coordinate.
2) Divide the points into two halves on the basis of X coordinates (Recurrsion).
3) Recurrsively call the function closestUtil on both the halves, which will in turn find the minimum distance between the pair of points from both the halves.
4) The minimum distance obtained from both the halves is compared and smallest among them is stored in d.
5) It can happen that the closest pair having minimum distance exists such that one pont is in the left half and the other point is in the right half. stripClosest() function is responsible to handle such cases.
