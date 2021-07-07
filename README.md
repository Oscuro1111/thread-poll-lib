# thread-poll

***Thread poll library in C for creating threads poll.***

> - ***It can handle larger incomming request load and manage threads sync in large systems too.***
> - ***No limit to thread worker number and use every bit of CPU cores to optimized way.***

# Usage example:

## **main.c**
```
#define MAX_POLL_THREAD 5

#include "../includes/thread_poll.h"
int main(int argc, char **argv)
{
	int i;
	const uint32_t MAX =MAX_POLL_THREADS;
	thr_arg arg[MAX_POLL_THREADS] = {
	    {.data = "wrk-1"},
	    {.data = "wrk-2"},
	    {.data = "wrk-3"},
	    {.data = "wrk-4"},
	    {.data = "wrk-5"},
	};

	worker_poll *w_poll = allocate_wrk_poll();

	worker_poll_init(w_poll,MAX);

	poll_worker *wrk;

	i = 0;
again:
	if (i == MAX){

		wrk_poll_free(w_poll);
		return 0;
	}
	
        //Get worker from worker poll worker.It will suspend process if  all worker busy and untile any  thread worker is free or thread available.
	wrk = get_worker_poll(w_poll);
	
	wrk->arg = &arg[i];
	wrk->work_func = work_thr;
	wrk->name = (char *)arg[i].data;
	
	//starting worker
	start_worker(w_poll, wrk);
	++i;
	goto again;

	return 0;
}
     


```
**Building**

>gcc ./lib/* -o bin main.c
