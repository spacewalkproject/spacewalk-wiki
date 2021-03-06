# Taskomatic Enhancements

## Requirements




 * schedule tasks on the satellite server
  * local commands (f.e. satellite-sync, spacewalk-repo-sync)
  * java tasks (f.e. todays tasks, remove duplicate profiles)
 * possibility to run the tasks periodically and at specific time
 * have the settings fully configurable (via WebUI and API)
 * task schedule per Satellite and per Organisation
 * to have pre-defined tasks after satellite installation
## Quartz



The heart of the scheduling will be the quartz library. It can schedule tasks at specified time, run them periodically. And ... it can use configuration directly from the database.
The idea is to create necessary tables for quartz (qrtz_job_listeners, qrtz_simple_triggers, qrtz_job_details, qrtz_locks, ...), and let quartz schedule tasks.
### Quartz job parameters



Because every task type requires different set of parameters and every task different parameter values, Quartz introduces a JobDataMap class, that can hold any number of (serializable) objects which you wish to have made available to the job instance when it executes.
### Quartz documentation

http://www.quartz-scheduler.org/docs/index.html

### Taskomatic communication

The quartz scheduler will run within the taskomatic process. It'll use a XMLRPM server to enable communication with WebUI and API from a different process (similar to cobbler and search-server communication).


Because we introduce a regular taskomatic API, the taskomatic handler will pass the API call to the taskomatic XMLRPC server and pass the response back.
The user authentication shall be done in the taskomatic handler and dis/allow the further communication.

Implemented API: https://fedorahosted.org/spacewalk/wiki/Features/TaskomaticAPI
### Data model



Preliminary concept of database tables:

 * rhnTaskoTask
  * contains basically only task types that is possible to run on the satellite
    * id
    * name
    * parallelizable

 * rhnTaskoSchedule
  * contains information when, how often rhnTaskoGroup shall be scheduled
  * (will be probably replaced by a default qrtz_ table)
    * id
    * next_run
    * periodicity
    * params

 * rhnTaskoTemplate
  * contains a list of rhnTaskoTasks that shall be scheduled within the same time
  * contains also information, that a task will be run only after the previous finishes/finishes successfully/fails
    * id
    * task_id
    * schedule_id
    * start_if
    * active_from
    * active_till

 * rhnTaskoRun
  * describes a concrete run of a rhnTaskoTask
  * contains information about what rhnTaskoRun and rhnTaskoTemplate it's an instance of
  * contains information when it started and ended and it's status
  * contains logs (std output, std error if available)
    * id
    * task_id
    * template_id
    * start_time
    * end_time
    * std_output
    * std_error
    * status
## Presentation

Scheduled tasks shall be visible on WebUI. Another page would display finished runs together with appropriate logs.

 * sat admin can see/edit his own tasks and see tasks of all org admins
 * an org admin can see/edit his own tasks (and read a rough list of sat admins' tasks, if he enables it)
## Scheduled commands

It's possible to schedule predefined commands only. (Running any command would be a high security risk.) User would choose from a drop-down menu a predefined task and can specify a parameter if needed.

## Satellite Installation/Upgrade

There're two parts:

 * schema changes, there's a need to drop actually used taskomatic tables and to create new tables
 * to pre-fill database with default task templates and default scheduling of these templates
    * these data should be deployed as a part of the database population process
## Task parallelization

Most probably there will be tasks scheduled at the same time (org admins won't have the right to each other's schedules). Some tasks can be run at the same time, some not. Most critical are the tasks of the same type. Because some tasks of the same type can be run in parallel (f.e. spacewalk-repo-sync) and some not (f.e. satellite-sync), there's a "parallelizable" property for every task type. If a task cannot be run, will posponed.

## History

It will be possible to check chronological history of task runs, runs of the same task type, within the same template within the organisation.

