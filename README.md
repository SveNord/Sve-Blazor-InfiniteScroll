 # Sve-Blazor-InfiniteScroll

Simplistic implementation of an infinite scroll component for Blazor.

![Main gif](/Sve-Blazor-InfiniteScroll-Examples/Content/Main2.gif)


### Installation
1. Install the [NuGet](https://www.nuget.org/packages/Sve.Blazor.InfiniteScroll/) package:

   ```
   > dotnet add package Sve.Blazor.InfiniteScroll
   
   OR
   
   PM> Install-Package Sve.Blazor.InfiniteScroll
   ```
   Use the `--version` option to specify a specific version to install.

   Or use the build in NuGet package manager of your favorite IDEA. Simply search for `Sve.Blazor.InfiniteScroll`, select a version and hit install.

2. Import the component:

   Add the following using statement `@using Sve.Blazor.InfiniteScroll.Components` to one of the following: 
   - For global import add it to your  `_Imports.razor` file
   - For a scoped import add  it to your desired Blazor component

3. Reference js interop file:
   
    Add `<script src="/_content/Sve.Blazor.InfiniteScroll/js/Observer.js"></script>` to your _Host.cshtml or your index.html

## Usage

Simply wrap the desired content in the `InfiteScroll` component and add any empty html element with a unique id after the your data elements: 

```csharp
<InfiniteScroll ObserverTargetId="observerTarget" ObservableTargetReached="(e) => FetchForecasts()">
	<ul>
    		@foreach (var forecast in forecasts)
        	{
        		<li class="list-group-item">@forecast.Date: @forecast.TemperatureC-@forecast.TemperatureF (@forecast.Summary)</li>
		}

        	@*The target element that we observe. Once this is reached the callback will be triggered.*@
        	<li class="list-group-item" id="observerTarget"></li>
    	</ul>
</InfiniteScroll>

@code {
    private List<WeatherForecast> forecasts = new List<WeatherForecast>();

    protected override async Task OnInitializedAsync()
    {
        await FetchForecasts(); // Initial data
    }

    private async Task FetchForecasts()
    {
        var fetchedForecasts = await ForecastService.GetForecastAsync(DateTime.Now, amount: 20);
        forecasts.AddRange(fetchedForecasts); // Make sure to use AddRange() to append the new items
    }
}
```

The `InfiniteScroll` component requires two properties:

- **ObserverTargetId**: The id of the observable html element

- **ObservableTargetReached**: The callback that will be triggered once the observable item is reached

  

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details
