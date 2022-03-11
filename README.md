# Csharp-project


## Introduction
This was a two week sprint for a Theatre Company in Portland OR. I was tasked at first to create a JSON file and having it deserialized in the sitesettings class. The stories following were to create a Cast Members CRUD where a cast member could be created, deleted, or edited. 

*Due to this being a team collaboration and doing work for a company not all code can be shared. I have included some of what I was able to complete in my 2 week sprint.

### JSON Deserializer
The first story to get me introduced into the project was to create a Json file that simply had the footer as "Copyright" with a value of 2021. This would be added as a footer on all pages and could add more information later on.
This is the deserializer code:
```
 public static SiteSettings ReadSiteSettings()
        {
            string filename = HttpContext.Current.Server.MapPath("~/SiteSettings.json");
            SiteSettings siteSettings1 = JsonConvert.DeserializeObject<SiteSettings>(File.ReadAllText(filename));
            return siteSettings1;
        }
```
### Models
With my next story I was tasked to create a Cast Members model. Using code first this needed to be crated before scaffolding so the properties would be set in the database under the table CastMembers.
```
   public class CastMember 
    {
            public int CastMemberId { get; set; }
            public string Name { get; set; }
            public int? YearJoined { get; set; }
            public position MainRole { get; set; }
            public string Bio { get; set; }
            //public byte[] Photo { get; set; }
            public bool CurrentMember { get; set; }
            public string Character { get; set; }
            public int? CastYearLeft { get; set; }
            public int? DebutYear { get; set; }
            public string ProductionTitle { get; set; }
    }

    public enum position
    {
        Actor,
        Director,
        Technician,
        StageManager,
        Other
    }
    
```
I created a public enum for position as it defined a seperate property.

Using MVC I created a Cast Members Controller.

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
In my next story I was taked to create 2 tables with bootstrap cards that you could click on to view the cast members details with edit and delete buttons that appeared over the cards on hover.

### Index

```
 <tr>
    @foreach (var item in Model)
    {
      <th>
        @item.ProductionTitle
      </th>


      <td>
        <div class="card-deck">
          <div class="card col-lg-6 mb-6" style="width: 18rem;">
            <a class="card-img-overlay" href="@Url.Action("Details", new { @id = item.CastMemberId })">
              <div class="card-img-overlay cast--card-overlay text-center --card">
                <div>
                  @Html.ActionLink("Edit", "Edit/5", new { id = item.CastMemberId }, new { @class = "btn btn-light fas fa-user-edit" })
                  @Html.ActionLink("Delete", "Delete", new { id = item.CastMemberId }, new { @class = "btn btn-light fas fa-user-slash" })
                </div>
              </div>
            </a>
            <img class="card-img" src="~/Content/images/Cast_Img_3.jpg" />
            <div class="card-body">
              <h5 class="card-person text-center">@item.Name</h5>
            </div>
          </div>
        </div>
      </td>
    }
  </tr>
</table>

<table class="table">

  <tr>
    @foreach (var item in Model)
    {
      <th>
        @item.ProductionTitle
      </th>
      <td>
        <div class="card-deck">
          <div class="card col-lg-6 mb-6" style="width: 18rem;">
            <a class="card-img-overlay" href="@Url.Action("Details", new { @id = item.CastMemberId })">
              <div class="card-img-overlay cast--card-overlay text-center --card">
                <div>
                  @Html.ActionLink("Edit", "Edit", new { id = item.CastMemberId }, new { @class = "btn btn-light fas fa-user-edit" })
                  @Html.ActionLink("Delete", "Delete", new { id = item.CastMemberId }, new { @class = "btn btn-light fas fa-user-slash" })
                </div>
              </div>
            </a>
            <img class="card-img" src="~/Content/images/Cast_Img_3.jpg" />
            <div class="card-body">
              <h5 class="card-person text-center">@item.Name</h5>
            </div>
          </div>
        </div>
      </td>
    }
  </tr>
</table>
```
The tables are meant to seperate Cast Members by the Production they starred in.

### Model View/ Controller
Of course I needed to sort and find castmembers starring in a production using LINQ. To do this I created a ModelView where I would pull from Name and Production Title to return to the view.

```
 public class CastMemberViewModel
    {
        public int CastMemberId { get; set; }
        public string Name { get; set; } //CastMember
        public string ProductionTitle { get; set; } //Production
    }
```

And the controller for the index
```
 public ActionResult Index()
        {
            //IQueryable<CastMemberViewModel> productionGroup = from castMember in db.CastMembers
            //                                                  group castMember
            //                by castMember.ProductionTitle into productions
            //                                                  select new CastMemberViewModel()
            //                                                  {
            //                                                      ProductionTitle = productions.Key
            //                                                  };
            //return View(productionGroup.ToList());

            var listCast = new List<CastMemberViewModel>();
            listCast = db.CastMembers.ToList().Select(c => new CastMemberViewModel
            {
                CastMemberId = c.CastMemberId,
                Name = c.Name,
                ProductionTitle = c.ProductionTitle
            }).ToList();

            var castList = listCast.Where(c => c.ProductionTitle.StartsWith("W"));
            
            return View(castList);
        }
```

### Skills Gained

While doing this sprint I learned a lot on MVC. Creating a MVC application and utilizing bootstrap for my index page was great practice for what I assume is what I could be doing in the near future. There were a few snags I overcame by reaching out to my team and seeking help when needed. Overall working on a project like this is a huge confidence boost. 


```
