---
layout: post
title: Quartz i Ninject
type: post
categories:
- C#
tags:
- Ninject
- Quartz .NET
permalink: "/quartz-i-ninject/"
image: 'https://sienicki.dev/assets/tiles/quartzIninject.png'
category: 'blog' 
introduction: Ninject Extensions Quartz
---
Hej,

Dziś miałem okazję po raz kolejny użyć [Quartz .NET](http://www.quartz-scheduler.net/documentation/quartz-2.x/tutorial/using-quartz.html). Dlatego też postanowiłem napisać nowego posta na temat wstrzykiwania zależności w job'ach.

Pewnie niesamowicie spłycając:

_[Quartz](http://www.quartz-scheduler.net/documentation/faq.html) jest to system planowania zadań który umożliwia łatwe tworzenie własnych zadań o ustalonej porze._

Kiedy pierwszy raz usłyszałem o tej bibliotece byłem pełen obaw o to czy to rzeczywiście działa jak należy. 
Po instalacji i przetestowaniu pierwszych "Jobów" byłem już dobrych myśli. 
Postanowiliśmy użyć tej biblioteki w naszym projekcie i zastępowaliśmy nasze joby Quartzowymi. 
Prawie na samym początku okazało się, że może być problem z wstrzykiwaniem zależności w konstruktory tych jobów.

Znalazłem wtedy paczkę [Ninject.Extensions.Quartz](http://www.nuget.org/packages/Ninject.Extensions.Quartz/) - nie ufajcie jej :wink: działało, ale po zaktualizowaniu w  projekcie Ninject do nowszej wersji - przestało.

To co robi rozszerzenie to rejestracja modułu QuartzNinjectModule w Ninject. 
Można zrobić to samemu i przechowywać te klasy w projekcie.

Rejestracja modułu w Ninject:
```csharp
kernel.Load(new QuartzNinjectModule());
```
Budowa modułu QuartzNinjectModule:
```csharp
public class QuartzNinjectModule : NinjectModule 
{ 
  public override void Load() 
  { 
      Bind<ISchedulerFactory>().To<NinjectSchedulerFactory>(); 
      Bind<IScheduler>()
          .ToMethod(c => c.Kernel
                          .Get<ISchedulerFactory>()
                          .GetScheduler())
          .InSingletonScope();       
  }
}
```
W module korzystamy z fabryki planera którą musimy sobie sami stworzyć:
```csharp
public class NinjectSchedulerFactory : StdSchedulerFactory 
{ 
  private readonly NinjectJobFactory _ninjectJobFactory; 
  
  public NinjectSchedulerFactory(NinjectJobFactory ninjectJobFactory) 
  { 
    _ninjectJobFactory = ninjectJobFactory; 
  } 
  
  protected override IScheduler Instantiate(QuartzSchedulerResources quartzSchedulerResources, QuartzScheduler quartzScheduler) 
  { 
    quartzScheduler.JobFactory = _ninjectJobFactory; 
    
    var scheduler = base.Instantiate(quartzSchedulerResources, quartzScheduler); 
    
    scheduler.Start();
    
    return scheduler;
  } 
}
```
I ostatnią klasą jaką musimy dodać do projektu to NinjectJobFactory:
```csharp
public class NinjectJobFactory : IJobFactory 
{ 
    private static readonly ILog Log = LogManager.GetLogger(typeof(NinjectJobFactory));
    private readonly IKernel _kernel; 
    
    static NinjectJobFactory() {}
    
    public NinjectJobFactory(IKernel kernel)
    {
      _kernel = kernel;
    }
    
    public IJob NewJob(TriggerFiredBundle bundle, IScheduler scheduler)
    {
        IJobDetail jobDetail = bundle.JobDetail;   
        Type jobType = jobDetail.JobType;
    
        try 
        {
          Log.Debug($"Producing instance of Job '{jobDetail.Key}', class={jobType.FullName}");
          
          return _kernel.Get(jobType) as IJob;
        } 
        catch (Exception ex)
        {
          throw new SchedulerException($"Problem instantiating class '{jobType.FullName}'", ex);
        }
  } 

  public void ReturnJob(IJob job) {} 
}
```
Po dodaniu tych klas do projektu, nasz HelloJob mający w konstruktorze zależność będzie działać o ile poprawnie zainicjujemy Scheduler.
```csharp
[DisallowConcurrentExecution] 
public class BreakerJob : IJob
{ 
    private readonly IUserDataDomain _userDataDomain; 
    
    public BreakerJob(IUserDataDomain userDataDomain) 
    { 
      _userDataDomain = userDataDomain; 
    } 
    
    public void Execute(IJobExecutionContext context) 
    { 
      // DO SOMETHING 
    }
}
```
Z reguły tworzę sobie klasę pomocniczą o nazwie JobScheduler którego konstruktor wygląda w ten sposób:
```csharp
public JobScheduler(IScheduler scheduler) 
{ 
    _scheduler = scheduler; RegisterJobs(); 
}
```
Poza rejestracją jobów umożliwiam uruchomienie IScheduler:
```csharp
public void Start() 
{ 
    _scheduler.Start(); 
}
```
I ostatnią rzeczą jaką robię to uruchomienie schedulera - JobScheduler stworzonego w naszym IoC.
```csharp
    var jobScheduler = _kernel.Get(); 
    jobScheduler.Start();
```