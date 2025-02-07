---
layout: post
title: Resource View in Blazor Gantt Chart Component | Syncfusion
description: Checkout and learn here all about Resource view in Syncfusion Blazor Gantt Chart component and more.
platform: Blazor
control: Gantt Chart
documentation: ug
---

# Resource view in Blazor Gantt Chart Component

To visualize tasks assigned to each resource in a hierarchical manner, you can set the [ViewType](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Gantt.SfGantt-1.html#Syncfusion_Blazor_Gantt_SfGantt_1_ViewType) property to `ResourceView` during initialization of the Gantt Chart. This view represents resources as parent records and their corresponding tasks as child records.

## Unassigned task

Unassigned tasks in the Gantt Chart refer to tasks that have not been assigned to any particular resource. These tasks are categorized under the label `Unassigned Task` and appear at the bottom of the Gantt Chart's data collection. The Gantt Chart's default behavior is to validate unassigned tasks during record creation, based on the task's [Resources](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Gantt.GanttResourceFields-1.html#Syncfusion_Blazor_Gantt_GanttResourceFields_1_Resources) mapping property in the data source. If a resource is subsequently assigned to an unassigned task, the task will be repositioned as a child task under the assigned resource.

## Resource task
A task assigned to a resource is termed a resource task and is displayed as a child task under the corresponding resource in the Gantt chart.

N> There is not support for Indent/Oudent in resource view Gantt Chart.

```cshtml
@using Syncfusion.Blazor.Gantt

<SfGantt DataSource="@TaskCollection" Height="450px" Width="800px" AllowRowDragAndDrop="true"
            Toolbar="@(new List<Object>() { "Add", "Cancel", "Update" , "Delete", "Edit", "CollapseAll", "ExpandAll" })" ViewType="ViewType.ResourceView">
    <GanttTaskFields Id="TaskId" Name="TaskName" StartDate="StartDate" EndDate="EndDate" Duration="Duration" Progress="Progress"
                        ParentID="ParentId" Work="Work" ResourceInfo="Resources" Dependency="Predecessor">
    </GanttTaskFields>
    <GanttResourceFields Group="ResourceGroup" Resources="@ResourceCollection" Id="ResourceId" Name="ResourceName" Unit="Unit" TResources="ResourceAlloacteData"></GanttResourceFields>
    <GanttLabelSettings RightLabel="Resources" TaskLabel="Progress" TValue="TaskData"></GanttLabelSettings>
    <GanttEditSettings AllowAdding="true" AllowDeleting="true" AllowEditing="true" AllowTaskbarEditing="true"></GanttEditSettings>
</SfGantt>
@code {
    private List<TaskData> TaskCollection { get; set; }
    private List<ResourceAlloacteData> ResourceCollection { get; set; }
    protected override void OnInitialized()
    {
        this.TaskCollection = GetTaskCollection();
        this.ResourceCollection = GetResources;
    }
    public class ResourceAlloacteData
    {
        public int ResourceId { get; set; }
        public string ResourceName { get; set; }
        public double Unit { get; set; }
        public string? ResourceGroup { get; set; }
    }
    public static List<ResourceAlloacteData> GetResources = new List<ResourceAlloacteData>()
    {
        new ResourceAlloacteData() { ResourceId= 1, ResourceName= "Martin Tamer", ResourceGroup="Planning Team"},
        new ResourceAlloacteData() { ResourceId= 2, ResourceName= "Rose Fuller", ResourceGroup="Testing Team" },
        new ResourceAlloacteData() { ResourceId= 3, ResourceName= "Margaret Buchanan", ResourceGroup="Approval Team" },
        new ResourceAlloacteData() { ResourceId= 4, ResourceName= "Fuller King", ResourceGroup="Development Team" },
        new ResourceAlloacteData() { ResourceId= 5, ResourceName= "Davolio Fuller", ResourceGroup="Approval Team" },
    };
    public class TaskData
    {
        public int TaskId { get; set; }
        public string TaskName { get; set; }
        public DateTime StartDate { get; set; }
        public DateTime EndDate { get; set; }
        public string Duration { get; set; }
        public int Progress { get; set; }
        public int? ParentId { get; set; }
        public string Predecessor { get; set; }
        public string Notes { get; set; }
        public double? Work { get; set; }
        public string TaskType { get; set; }
        public List<ResourceAlloacteData> Resources { get; set; }
    }
    public static List<TaskData> GetTaskCollection()
    {
        List<TaskData> Tasks = new List<TaskData>() {
            new TaskData() { TaskId = 1, TaskName = "Project initiation", StartDate = new DateTime(2022, 03, 28), EndDate = new DateTime(2022, 07, 28), TaskType ="FixedDuration", Work=128, Duration="4" },
            new TaskData() { TaskId = 2, TaskName = "Identify Site location", StartDate = new DateTime(2022, 03, 29), Progress = 30, ParentId = 1, Duration="5", Work=16, Resources = new List<ResourceAlloacteData>(){ new ResourceAlloacteData() { ResourceId=1, Unit=70} } },
            new TaskData() { TaskId = 3, TaskName = "Site Analyze", StartDate = new DateTime(2022, 03, 29), Progress = 50, ParentId = 1, Duration="5", Work=16, Resources = new List<ResourceAlloacteData>(){ new ResourceAlloacteData() { ResourceId=1, Unit=70} } },
            new TaskData() { TaskId = 4, TaskName = "Perform soil test", StartDate = new DateTime(2022, 03, 29), Resources = new List<ResourceAlloacteData>(){ new ResourceAlloacteData() { ResourceId=3} }, ParentId = 1, Work=96, Duration="6", TaskType="FixedWork", Predecessor="2FS", Progress=40 },
            new TaskData() { TaskId = 5, TaskName = "Soil test approval", StartDate = new DateTime(2022, 03, 29), Duration="4", Progress = 30, ParentId = 1, Resources = new List<ResourceAlloacteData>(){ new ResourceAlloacteData() { ResourceId=4} }, Work=16, TaskType="FixedWork" },
            new TaskData() { TaskId = 6, TaskName = "Project estimation", StartDate = new DateTime(2022, 03, 29), EndDate = new DateTime(2022, 04, 2), TaskType="FixedDuration", Duration="7", Progress=40, Work=50 },
            new TaskData() { TaskId = 7, TaskName = "Develop floor plan for estimation", StartDate = new DateTime(2022, 03, 29), Duration = "9", Progress = 30, ParentId = 5, Resources = new List<ResourceAlloacteData>(){ new ResourceAlloacteData() { ResourceId=4, Unit=30} }, Work=30, TaskType="FixedDuration",  Predecessor= "4FS" },
            new TaskData() { TaskId = 8, TaskName = "List materials", StartDate = new DateTime(2022, 04, 01), Duration = "6", Progress = 30, ParentId = 5, TaskType="FixedWork", Work=48, Resources = new List<ResourceAlloacteData>(){new ResourceAlloacteData() { ResourceId=5} },  },
       };
        return Tasks;
    }
}
```

## Resource OverAllocation

When a resource is assigned more work than they can complete within their available time in a day, it is referred to as overallocation. The available working time for resources to complete tasks in a day is calculated based on the [GanttDayWorkingTime](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Gantt.GanttDayWorkingTimeCollection.html#Syncfusion_Blazor_Gantt_GanttDayWorkingTimeCollection_DayWorkingTime) property and the resource unit.

To highlight the range of overallocation dates with a square bracket, you can enable this feature by setting the [ShowOverallocation](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Gantt.SfGantt-1.html#Syncfusion_Blazor_Gantt_SfGantt_1_ShowOverallocation) property to `true`.

```cshtml

@using Syncfusion.Blazor.Gantt

<SfGantt ShowOverallocation="true" DataSource="@TaskCollection" Height="450px" Width="800px" AllowRowDragAndDrop="true"
            Toolbar="@(new List<Object>() { "Add", "Cancel", "Update" , "Delete", "Edit", "CollapseAll", "ExpandAll" })" ViewType="ViewType.ResourceView">
    <GanttTaskFields Id="TaskId" Name="TaskName" StartDate="StartDate" EndDate="EndDate" Duration="Duration" Progress="Progress"
                        ParentID="ParentId" Work="Work" ResourceInfo="Resources" Dependency="Predecessor">
    </GanttTaskFields>
    <GanttResourceFields Group="ResourceGroup" Resources="@ResourceCollection" Id="ResourceId" Name="ResourceName" Unit="Unit" TResources="ResourceAlloacteData"></GanttResourceFields>
    <GanttLabelSettings RightLabel="Resources" TaskLabel="Progress" TValue="TaskData"></GanttLabelSettings>
    <GanttEditSettings AllowAdding="true" AllowDeleting="true" AllowEditing="true" AllowTaskbarEditing="true"></GanttEditSettings>
</SfGantt>

@code {
    private List<TaskData> TaskCollection { get; set; }
    private List<ResourceAlloacteData> ResourceCollection { get; set; }
    protected override void OnInitialized()
    {
        this.TaskCollection = GetTaskCollection();
        this.ResourceCollection = GetResources;
    }
    public class ResourceAlloacteData
    {
        public int ResourceId { get; set; }
        public string ResourceName { get; set; }
        public double Unit { get; set; }
        public string? ResourceGroup { get; set; }
    }
    public static List<ResourceAlloacteData> GetResources = new List<ResourceAlloacteData>()
    {
        new ResourceAlloacteData() { ResourceId= 1, ResourceName= "Martin Tamer", ResourceGroup="Planning Team"},
        new ResourceAlloacteData() { ResourceId= 2, ResourceName= "Rose Fuller", ResourceGroup="Testing Team" },
        new ResourceAlloacteData() { ResourceId= 3, ResourceName= "Margaret Buchanan", ResourceGroup="Approval Team" },
        new ResourceAlloacteData() { ResourceId= 4, ResourceName= "Fuller King", ResourceGroup="Development Team" },
        new ResourceAlloacteData() { ResourceId= 5, ResourceName= "Davolio Fuller", ResourceGroup="Approval Team" },
    };
    public class TaskData
    {
        public int TaskId { get; set; }
        public string TaskName { get; set; }
        public DateTime StartDate { get; set; }
        public DateTime EndDate { get; set; }
        public string Duration { get; set; }
        public int Progress { get; set; }
        public int? ParentId { get; set; }
        public string Predecessor { get; set; }
        public string Notes { get; set; }
        public double? Work { get; set; }
        public string TaskType { get; set; }
        public List<ResourceAlloacteData> Resources { get; set; }
    }
    public static List<TaskData> GetTaskCollection()
    {
        List<TaskData> Tasks = new List<TaskData>() {
            new TaskData() { TaskId = 1, TaskName = "Project initiation", StartDate = new DateTime(2022, 03, 28), EndDate = new DateTime(2022, 07, 28), TaskType ="FixedDuration", Work=128, Duration="4" },
            new TaskData() { TaskId = 2, TaskName = "Identify Site location", StartDate = new DateTime(2022, 03, 29), Progress = 30, ParentId = 1, Duration="5", Work=16, Resources = new List<ResourceAlloacteData>(){ new ResourceAlloacteData() { ResourceId=1, Unit=70} } },
            new TaskData() { TaskId = 3, TaskName = "Site Analyze", StartDate = new DateTime(2022, 03, 29), Progress = 50, ParentId = 1, Duration="5", Work=16, Resources = new List<ResourceAlloacteData>(){ new ResourceAlloacteData() { ResourceId=1, Unit=70} } },
            new TaskData() { TaskId = 4, TaskName = "Perform soil test", StartDate = new DateTime(2022, 03, 29), Resources = new List<ResourceAlloacteData>(){ new ResourceAlloacteData() { ResourceId=3} }, ParentId = 1, Work=96, Duration="6", TaskType="FixedWork", Predecessor="2FS", Progress=40 },
            new TaskData() { TaskId = 5, TaskName = "Soil test approval", StartDate = new DateTime(2022, 03, 29), Duration="4", Progress = 30, ParentId = 1, Resources = new List<ResourceAlloacteData>(){ new ResourceAlloacteData() { ResourceId=4} }, Work=16, TaskType="FixedWork" },
            new TaskData() { TaskId = 6, TaskName = "Project estimation", StartDate = new DateTime(2022, 03, 29), EndDate = new DateTime(2022, 04, 2), TaskType="FixedDuration", Duration="7", Progress=40, Work=50 },
            new TaskData() { TaskId = 7, TaskName = "Develop floor plan for estimation", StartDate = new DateTime(2022, 03, 29), Duration = "9", Progress = 30, ParentId = 5, Resources = new List<ResourceAlloacteData>(){ new ResourceAlloacteData() { ResourceId=4, Unit=30} }, Work=30, TaskType="FixedDuration",  Predecessor= "4FS" },
            new TaskData() { TaskId = 8, TaskName = "List materials", StartDate = new DateTime(2022, 04, 01), Duration = "6", Progress = 30, ParentId = 5, TaskType="FixedWork", Work=48, Resources = new List<ResourceAlloacteData>(){new ResourceAlloacteData() { ResourceId=5} },  },
       };
        return Tasks;
    }
}

```

## Limitations

* The resource view in the Gantt Chart does not support assigning multiple resources to a single task.
* Editing of resource records is not supported in the resource view of the Gantt Chart.


Gantt Chart Resource view

![Blazor Gantt Chart Resource view](images/blazor-gantt-chart-resource-view.png)