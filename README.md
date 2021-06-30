# HITDAY23
package hit.day23;
public class OptimisticLock {
	public static void main(String[] args) {
		CounsellingHall university=new CounsellingHall();
		Thread imran=new Thread(new CounsellingJob(university),"imran");
		Thread secondtaqi=new Thread(new CounsellingJob(university),"secondtaqi");
		imran.start();
		secondtaqi.start();
	}
}
class CounsellingJob implements Runnable{
	public CounsellingJob(CounsellingHall university) {
		this.university=university;
	}
	CounsellingHall university;
	@Override
	public void run() {
	//	synchronized (university) {
		// TODO Auto-generated method stub
		if(Thread.currentThread().getName().equals("imran")) {
			university.table1();university.table2();
		}
		else if(Thread.currentThread().getName().equals("secondtaqi")){
			university.water();
		}
	//	}
	}
}
class CounsellingHall{
	synchronized public void table1() {
		System.out.println("entered table1...:"+Thread.currentThread().getName());
		try {Thread.sleep(5000);}catch(Exception e) {}
	}
	synchronized public void table2() {
		System.out.println("entered table2...:"+Thread.currentThread().getName());
	}
	public void water() {
		System.out.println("entered water area to drink water....:"+Thread.currentThread().getName());
	}
}


package hit.day23;
public class TwoThreadDemo {
	public static void main(String[] args) {
		ReservationCounter central=new ReservationCounter();
		Thread taqi=new Thread(new TicketBooking(central, 1000),"taqi");
		Thread imran=new Thread(new TicketBooking(central,500),"imran");
		
		taqi.start();
		imran.start();
	}
}
class TicketBooking implements Runnable{
	ReservationCounter central;int amt;
	public TicketBooking(ReservationCounter central,int amt) {
		this.central=central;
		this.amt=amt;
	}
	@Override
	public void run() {
		synchronized(central) {
			central.bookTicket(amt);
			central.getChange();
		}
	}
}
class ReservationCounter{
	int amt;
	public void bookTicket(int amt) {
		this.amt=amt;
		Thread t=Thread.currentThread();
		String name=t.getName();
		System.out.println("Ticket booked by Mr...:"+name);
		System.out.println("Amount brought by..:"+name+": is: "+amt);
		try {Thread.sleep(5000);}catch(Exception e) {}
	}
	public void getChange() {
		Thread t=Thread.currentThread();
		String name=t.getName();
		System.out.println("Change took by Mr...:"+name);
		System.out.println("Change Amount for..:"+name+": is: "+(this.amt-100));		
	}
}


package hit.day22;
//Parallel processing
//1. Performance 2. Asynchronous Programming
//How to create Threads ?
//We create thread using Thread class and using Executor Framework.
public class ThreadRevision {
	public ThreadRevision() {
		Thread t=new Thread(new ThreadJob());
		t.start();
	}
	public static void main(String[] args) {
		//How to capture the main thread
		//All java programs run in main thread by default.
		new ThreadRevision();
		Thread t=Thread.currentThread();
		System.out.println(t);
		//lazy for loop
		for(int i=0;i<5;i++) {
			try {
				Thread.sleep(1000);
			}catch(Exception e) {}
			System.out.println(i);
		}
	}
}
class ThreadJob implements Runnable{
	@Override
	public void run() {
		System.out.println("child thread is executed....");
	}
}
