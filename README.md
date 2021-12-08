<!-- Output copied to clipboard! -->


<p style="color: red; font-weight: bold">>>>>>  gd2md-html alert:  ERRORs: 0; WARNINGs: 1; ALERTS: 15.</p>
<ul style="color: red; font-weight: bold"><li>See top comment block for details on ERRORs and WARNINGs. <li>In the converted Markdown or HTML, search for inline alerts that start with >>>>>  gd2md-html alert:  for specific instances that need correction.</ul>

<p style="color: red; font-weight: bold">Links to alert messages:</p><a href="#gdcalert1">alert1</a>
<a href="#gdcalert2">alert2</a>
<a href="#gdcalert3">alert3</a>
<a href="#gdcalert4">alert4</a>
<a href="#gdcalert5">alert5</a>
<a href="#gdcalert6">alert6</a>
<a href="#gdcalert7">alert7</a>
<a href="#gdcalert8">alert8</a>
<a href="#gdcalert9">alert9</a>
<a href="#gdcalert10">alert10</a>
<a href="#gdcalert11">alert11</a>
<a href="#gdcalert12">alert12</a>
<a href="#gdcalert13">alert13</a>
<a href="#gdcalert14">alert14</a>
<a href="#gdcalert15">alert15</a>

<p style="color: red; font-weight: bold">>>>>> PLEASE check and correct alert issues and delete this message and the inline alerts.<hr></p>




1. Multi-Object Tracking (MOT)

    Multi-Object Tracking can be analyzed through five main procedures as following:

* Determine the original frames.
* Obtain object detection bounding by using object detectors such as Faster R-CNN, YOLO, SSD.
* Choose objects from the corresponding object bounding then select the characteristic.
* Compute similarity, that is, the matching degree between previous and current frames.
* Link the data and assign identity to objects.
2. Simple Online and Realtime Tracking (SORT)

	

<p id="gdcalert1" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image1.jpg). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert2">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image1.jpg "image_tooltip")



    SORT is an object detection algorithm based on Faster R-CNN, combined with Kalman filter and Hungarian methods to improve tracking speed of multi objects sharply and approaches the accuracy of SOTA (State of the Art). The algorithm defines the movement status as eight vectors of normal distribution.


    The Kalman filter algorithm consists of two procedures, predicting and updating. After the movement of objects, predicting is to forecast object location and speed of object in current frame compared with parameters such as location and speed of previous frame. Updating is designed to renew current status by linear computation of the prediction and observation. 


    The Hungarian algorithm addresses the assignment problem by computing the similarity to obtain a similarity matrix. The solution of the similarity matrix determines the truly matching objects between previous and current frames.


    SORT builds the similarity matrix with intersection over union (IOU) to achieve better performance on time.


    Detections are object boundings from object detectors. Track is a section of locus. The core of the SORT algorithm shown in the figure above includes matching procedure,  predicting and updating procedures of Kalman filter.


    The result from the IOU matching between detections from detector and tracks from Kalman filters is divided into following parts:



* Unmatched Tracks, which means the detection fails to match any tracks. The identity of unmatched track will be discarded if unmatched track status sustains 

<p id="gdcalert2" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: equation: use MathJax/LaTeX if your publishing platform supports it. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert3">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

frames.
* Unmatched Detections, which means all tracks fail to match detection and the detection will be assigned a new track. 
* Matched Track.
3. Deep SORT

    Deep SORT decreases identity switch times by adding appearance information and ReID domain model to extract characteristics.


    

<p id="gdcalert3" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image2.jpg). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert4">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image2.jpg "image_tooltip")



    From the chart, we can see that Deep SORT just adds matching cascade and new track confirmation based on SORT, that is, using Hungarian algorithm to match predicted tracks to detections in the frame (cascade matching and IOU matching) between Kalman’s predicting and updating process.


    

<p id="gdcalert4" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image3.jpg). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert5">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image3.jpg "image_tooltip")



    The part above the dotted line is to obtain the cost matrix gate matrix. The cost matrix measures similarity using the appearance model and movement model expressed by Cosine distance and Mahalanobis distance. Gate matrix is to limit the extremely large value of the cost matrix.


    The part below the dotted line is to realize the matching cascade, which is a cycle matching tracks to detections from the tracks that missing age equals from 0 to 70. The track without loss will be matched priorly, before the track which lost for a while. The cycle can retrieve the obscured objects and decrease the times of ID switch of reappearing objects.


    The results of matching can be discussed as below:

1. Matched Tracks, that is, detections match tracks. It is common to the continuously tracked objects that can be matched by either previous or current frame.
2. Unmatched Detections, that is, there is a new object, where detection can not be found in the previous track.
3. Unmatched Tracks, that is, the continuous tracked objects exceed the figure boundaries so that track fails to match any detections.
4. There is a special situation left, which is the occlusion. The track of occluded objects also can not match detection due to temporary disappearance. After reappearance of occluded objects, the ID should not be changed, which decreases ID switch by cascade matching.
4. Deep SORT algorithm

    

<p id="gdcalert5" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image4.jpg). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert6">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image4.jpg "image_tooltip")


    1. Deep SORT is the main module calling for other modules. There are three modules invoked by Deep SORT: ReID module, extracting appearance characteristics, Track module, storing track status information as a unit, Tracker module, containing Kalman filter and Hungarian algorithm.
    2. Classes
        1. Class Detection is designed for storing the detection boundings obtained from object detectors, including upper left corner location, the height and width of boundings, confidence of corresponding bounding box, the embedding obtained from ReID.
        2. Class Track is used to save track information. Mean and covariance represents the location and speed information of the bounding box, and track_id is the identity assigned to the track. There are three status of state as below:
* Tentative: Uncertainty. It is assigned when a track initialization happens. If the object is detected continuously for n_init frames, the status is transferred into confirmed status. Otherwise, if the object under tentative status can not match any detection, the status will be transferred to deleted.
* Confirmed: Certainty. It represents the track is in matched status, but the status will turn to deleted if matching failure continues max age frames.
* Deleted: The track is invalid.

            Max_age represents the life span of a track. Time_since_update is a counter which plus one with invoking predict function and becomes zero with invoking update function. If time_since_update exceeds max age, this track will be deleted from the tracker list.


            Hits records the calling times in transferring from tentative to confirmed, which plus one with invoking update function. If hits is larger than n_init, that is, there are continuous n_init frames satisfying matching, the status will be transferred from tentative to confirmed.


            Feature lists is designed to address characteristics matching in reappearance problems. Budget variable is to limit the list length, just saving the latest budget number of features and discarding the older ones.


            

<p id="gdcalert6" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image5.jpg). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert7">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image5.jpg "image_tooltip")


        3. Class ReID

            ReID net is an independent object detection and tracking module, which is used to extract features from corresponding bounding boxes to obtain embedding with fixed dimensions for similarity computation.

        4. Class NearNeighborDistanceMetric
        5. Class Tracker

            Tracker stores all the track information. This class is responsible for first frame’s initialization, Kalman filter predicting and updating, cascade matching, IOU matching and so on.

    3. Gate matrix

        Gate matrix is to limit the cost matrix measuring the distance between Kalman filter state distribution and observation. The distance in the cost matrix is the appearance similarity between Track and Detection. It is the solution to avoid the mistake of using a single track to match two similar appearance detection that takes two detection respectively with the track into calculation of Mahalanobis distance with gating_threshold and discarding the detection with larger distance.

    4. Kalman Filter

        The mean and covariance of track are required in Deep SORT. The mean is expressed as a eight-dimension vector 

<p id="gdcalert7" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: equation: use MathJax/LaTeX if your publishing platform supports it. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert8">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

. The (x,y) is the center of bounding, a is aspect ratio, h is height, and the last four values are velocity in the corresponding directions. The covariance measures the uncertainty of the object location, expressed as an eight by eight diagonal matrix. The larger the value of the matrix, the more uncertain the object is.


        The main procedures of Kalman filter are as follows:

1. Predict the status of the next frame using the status information of the current frame.
2. Obtain the observation, that is, the detection bounding from the object detector.
3. Update the prediction and observation.

		Prediction contains two formulas as follows:



1. 

<p id="gdcalert8" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image6.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert9">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image6.png "image_tooltip")
, which F is the status transfer matrix as follows:

            

<p id="gdcalert9" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image7.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert10">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image7.png "image_tooltip")


2. 

<p id="gdcalert10" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image8.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert11">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image8.png "image_tooltip")
, which P is the covariance of the current frame and Q is the motion estimation error of the Kalman filter, representing the uncertainty.

		Formulas in updating are as follows:



<p id="gdcalert11" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image9.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert12">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image9.png "image_tooltip")



        , where z is the mean of detection without changing values, the status is [cx,cy,a,h], H is the observation matrix mapping the mean vector of track into detection space. The result is the mean error of detection and track.



<p id="gdcalert12" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image10.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert13">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image10.png "image_tooltip")



        , where R is a four by four noise matrix of the object detector. The values on the diagonal are the noise of the center coordinates, width, heights.



<p id="gdcalert13" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image11.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert14">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image11.png "image_tooltip")



        , which calculates the Kalman amplifier, that is, the weight measuring estimation error.



<p id="gdcalert14" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image12.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert15">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image12.png "image_tooltip")



        , which is used to update the mean vector.



<p id="gdcalert15" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image13.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert16">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image13.png "image_tooltip")



        , which is used to update the covariance matrix.
