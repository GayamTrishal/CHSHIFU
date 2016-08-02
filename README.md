# PROBLEM STATEMENT
# https://www.codechef.com/LOCJUN16/problems/CHSHIFU/

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
