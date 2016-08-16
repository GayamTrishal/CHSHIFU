# PROBLEM STATEMENT
Chef Shifu wanted to celebrate the success of his new restaurant with all his employees. He was willing to host a party and he had decided the location of the party as well. However, Chef Shifu was a shy person and wanted to communicate with the least possible employees to inform them about the party, and that these employees could inform their friends.
Note that an employee could only inform his/her immediate friends about the party, not his/her friends’ friends.
Chef Shifu has a list of all the friendships among his employees. Help him find the minimum number of employees he should inform, so that every employee knows about the celebration party.
Input

First line contains a single integer T - the total number of testcases.

T testcases follow. For each testcase:

The first line contains 2 space-separated integers N and M - the total number of employees working under Chef Shifu and the number of friendship relations.

M lines follow - each line contains 2 space-separated integers u and v, indicating that employee u is a friend of employee v and vice-versa.

The employees are numbered from 1 to N, and each employee is assigned a distinct integer.

Output

For each testcase, print the minimum number of employees to be informed on a new line.
Constraints

Subtask 1: 5 points

    1 ≤ T ≤ 5

    1 ≤ N ≤ 4

    0 ≤ M ≤ N*(N-1)/2


Subtask 2: 35 points

    1 ≤ T ≤ 5

    1 ≤ N ≤ 15

    0 ≤ M ≤ N*(N-1)/2


Subtask 3: 60 points

    1 ≤ T ≤ 5

    1 ≤ N ≤ 20

    0 ≤ M ≤ N*(N-1)/2


Example

Input

    2
    3 3
    1 2
    2 3
    1 3
    4 3
    1 2
    2 3
    3 4

Output

    1
    2

Explanation

In testcase 1, since every employee is a friend of every other employee, we just need to select 1 employee.

In testcase 2, selecting employees 2 and 4 would ensure that all 4 employees are represented.
Similarly, selecting employees 1 and 3 would also ensure that all 4 employees are selected.
In both cases, we must select 2 employees in the best case. 

*******************************************************************************************************************************

# SOLUTION

    #include <iostream>
    #include <cmath>
 
    using namespace std;
 
    int recurse(int *u,int *v,int n,int m,int *p,int x,int **z);
     
    int main()
    {
        ios_base::sync_with_stdio(false);
        int t,n,m,i,value,j,**z;
        cin>>t;
        while(t--)
        {
            cin>>n>>m;
            int u[m],v[m],p[n];
            z=new int*[n];
            for(i=0;i<n;i++)
            {   
                p[i]=0;
                z[i]=new int[n];
                for(j=0;j<n;j++)
                    z[i][j]=0;
            }
            for(i=0;i<m;i++)
            {
                cin>>u[i]>>v[i];
                z[u[i]-1][v[i]-1]=1;
                z[v[i]-1][u[i]-1]=1;
            }
            value=recurse(u,v,n,m,p,-1,z);
            cout<<value<<"\n";
        }
        delete z;
        return 0;
    }
    int recurse(int *u,int *v,int n,int m,int *p,int x,int **z)
    {
        int found=0,q,i,d[n],j,t=0;
        for(j=0;j<n;j++)
            d[j]=p[j];
        if(x>=0)
        {
            if(d[x]==0)
            {
                t++;
                d[x]=2;
            }
            if(d[x]==1)
                d[x]=2;
            for(i=0;i<n;i++)
            {
                if(z[x][i]==1)
                {
                    if(d[i]==0)
                    {
                        d[i]=1;
                        t++;
                    }
                }
            }
            if(t==0)
                return 30;
        }
        for(i=n-1;i>=0;i--)
        {
            if(d[i]==0)
            {
                found=1;
                break;
            }
        }
        if(found==0)
            return 0;
        q=30;
        for(i=x+1;i<n;i++)
            q=min(q,1+recurse(u,v,n,m,d,i,z));
        return q;
    }
