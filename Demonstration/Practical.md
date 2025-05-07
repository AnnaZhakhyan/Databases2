# MongoDB Demonstration

## Creating a Cluster
In order to create a cluster, you first have to create a account on [MongoDB atlas](https://www.mongodb.com/products/platform/atlas-database)
After creating an Account by clicking the ```create cluster``` button within the link, you will be forwarded to an ```Overview ``` page. 
</br>
This is normally where you can see already existing clusters. In your case however, this should still be empty. Here you can see a ```create cluster``` button. By clicking it you will be forwarded to a window trying to sell you a chargeable plan. Click ```discard draft``` and select free, since a free cluster is ideal for experimenting in a limited sandbox. Now click ```create cluster```.
</br>
Follow the instructions by creating an account and adding your local ip address. Afterwards click on create.
</br>
Now, in the Overview you should see your Cluster with the name ```Cluster0``` - if you did not rename it - 
Congratulations! you have created your first cluster with MongoDB.

## Connecting to the Cluster

In order to connect to the cluster, here, I am using the [MongoDB Compass Software](https://www.mongodb.com/try/download/compass), however, everything explained here can be done inside of Atlas. </br>
An alternative is a [command-line tool](https://www.mongodb.com/try/download/database-tools), which is also included in the Compass Software and can be accessed inside of it. </br>
After downloading Compass -optional- , return to the Atlas Overview and select ```connect```. Here, you can see the different ways of connecting to the Cluster including explanation on how to connect using them. </br>
Keep in mind that you have to replace the whole ``` <db_username> ``` and ``` <db_password> ``` including the ```<>``` when pasting into Compass. This is the username and password you have created earlier, in step one when creating the cluster. Once you replaced these and clicked connect, you should be able to see the cluster the the left compass application. Congratulations! You have now connected your Closter to Compass.  </br>

## Filling the Cluster with data
In order to now get data into your cluster, you once more return to Atlas and select ``` Browse Collections ``` inside the ```Cluster 0``` Overview. Now, you should see two buttons: ``` Load a Sample Dataset ``` and ``` Add My Own Data ```. 
Select the ``` Load a Sample Dataset ``` option. After again click the option ```Load Sample Data```. Now it will load, a short time. Once finished, you can see an overview of the data in Atlas. If you now click ```Refresh```on the top right in Compass, you can see all the different sample data to the left. 


## Sharding a Cluster
This is unfortunately a paid feature, but can is explained [here](https://www.mongodb.com/docs/manual/tutorial/deploy-shard-cluster/#enable-sharding-for-a-database) in detail

## Aggregation Pipelines
Even though it can be done just as well in Atlas, I will use Compass for this. Everything remains the same on Atlas. 
First select one of the sample datasets recently added, like ```sample_supplies```. Now select a collection, in this case ```sales```. You now see all Documents in the Collection. Above, in the search field you can enter queries like ```{ "items.price": { $gt: 100 } }``` to in this case, filter for all sales above 100. But since simple CRUD is of no interest for this Demonstration, we will select the ```Aggregations``` tab.
</br></br>
For our example we will query for total sales per item.
 Below the preview you see a button ``Add Stage``. 
 Click it and in the dropdown menu select ```$unwind```. 
 Unwind deconstructs the `items` array in each document, creating a separate document for each item.
 Delete everything from the window and put ```"$items"``` in there. </br>
 With the preview activated you should see the results to the right. 
</br></br>
Add another stage and select ```$group``` in the dropdown, which groups the unwound documents and for each group execute code. Here we insert:</br>

```
{
  _id: "$items.name",
  totalSold: {
    $sum: "$items.quantity"
  }
}
```
</br>
Here we determine to group the results by item names and calculate their total sales. However, this still does not look nice with the _id and totalSold, which is why we are creating another stage, and select ```$project``` from the dropdown. $project reshapes the output document to include or exclude certain fields for clarity. Here we fill in </br>

```
{
  _id: 0,
  item: "$_id",
  sales: "$totalSold"
}
```
</br>
Now the preview results already look better with item and sales. 
However, you will notice, the results are still unsorted, so we add another stage and select ```$sort`` and enter this in the field:</br>

```
{
  sales: -1
}
```
</br>
the -1 sorts in descending order, a 1 in ascending. Alternatively totalSold also has the same effect.  As you can see in the preview, the results are now nicely formatted and sorted. Before clicking ```run `` to see all results, save the queries and name it. 
</br></br>
To get information helping you to optimise queries you can select Explain.
</br></br>
Congratulations! You now have created and saved an aggregation.

</br></br>
## Geospatial Data

Now in compass, we will click on ```sample_geospatial```, then ```shipwrecks``` collection and ```Open MongoDB Shell```.
With the prewritten query you can just type in enter, to execute it and observe the result. Here you can see different Documents for shipwrecks.</br></br></br>

on top in the search field, you can now query as example for all shipwrecks around a certain point using </br>

```
{
  coordinates: {
    $near: {
      $geometry: {
        type: "Point",
        coordinates: [-79.9081268, 9.3547792]
      },
      $maxDistance: 1000
    }
  }
}
```

</br>If you go the the Schema tab you have a Map view. In here you can also enter a geospatial query. 
</br></br>
Another example to visualise is a sphere </br>
```
{coordinates: {$geoWithin:
    { $centerSphere: [ 
        [ -80.79224357496888, 27.574472392552394 ], 
        0.0903922119472032 ]}
    }
} 
```

</br>or a polygon</br>

```
{
  "coordinates": {
    $geoWithin: {
      $geometry: {
        type: "Polygon",
        coordinates: [
          [
            [-80.0, 10.00], [ -80.0, 9.00], [ -79.0, 9.0], [ -79.0, 10.00 ], [ -80.0, 10.0 ]
          ]
        ]
      }
    }
  }
}
```
</br>The official tutorial on geospatial data can be found [here](https://www.mongodb.com/docs/manual/geospatial-queries/)

</br>

### resources
https://www.mongodb.com/docs/atlas/getting-started/
https://www.mongodb.com/docs/atlas/sample-data/
https://www.mongodb.com/docs/atlas/atlas-ui/collections/#shard-a-collection

