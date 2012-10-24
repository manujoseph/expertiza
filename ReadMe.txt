Refactoring done

1. methods 'get_percentage_reviews_completed' and get_total_reviews_completed_by_type_and_date'in assignment.rb

These methods query on the reviews performed for a particular assignment. An assignment is mapped to a review through a response map and a review is stored as a response. The foreign key in response_map is the review_object_id. The assignment_id is stored in that field if the review is for an assingment. Strictly speaking these functions do not directly relate to creation and maintenance of an assignment. So according to object oriented principles, these functions should not be part of assignment.rb file.

Refactoring done
________________

For get_percentage_reviews_completed, we added a function called get_percentage_reviews_completed to Response model which will return the percentage of reviews completed for a given review_object_id. This function now directly relates the response class as it processes responses. Then all calls to this function of the assignment class were replaced with a call to Response.get_percentage_reviews_completed but passing assignment.id this time. You can see this by verifying changes to assignment.rb and response.rb

Same way get_total_reviews_completed_by_type_and_date was moved from assignment.rb to response.rb. It now accepts a review_object_id parameter. All calls to assignment.get_total_reviews_completed_by_type_and_date were replaced with calls to Response.get_total_reviews_completed_by_type_and_date.

Verification
____________

Code can be verified in the github commit

https://github.com/manujoseph/expertiza/commit/d73683ee0447d23fa5d49439b9af9baa13121790

The above functions would be moved from assignment.rb to response.rb and all the usages would be changed as well.

As on expertiza all pages where reviews of assignments are displayed should remain unchanged

3. Other methods in assignment.rb that should not belong to this file

The methods get_average_score and get_score_distribution in assignment.rb relate to reviews and are not related directly to assignments. So these methods should be present in the response.rb file rather than assignment.rb file.

Refactoring done
________________

Similar to the refactoring 1 above, get_average_score and get_score_distribution were moved to response.rb file and were made generic. These methods now accept a review_object_id to query against.

All calls to these functions via an assignment object were replaced with a call to reponse class passing the assignment id.

As a consequense of the above changes, we no longer need the :response_maps assication to be present in assignment.rb. So we have removed this as well. Now an assignment object need carry only less state than before.

Verification
____________

Code can be verified in the github commit

https://github.com/manujoseph/expertiza/commit/d73683ee0447d23fa5d49439b9af9baa13121790

The above functions would be moved from assignment.rb to response.rb and all the usages would be changed as well.

As on expertiza all pages where reviews of assignments are displayed should remain unchanged

8. Also apply other refactorings such as Rename variable, Rename method to give the variables and methods more meaningful names

Typo in submit_hyperlinks method in assignment_participant.rb corrected (commit: https://github.com/manujoseph/expertiza/commit/374d442b3a4867531877e9755c2ab14c30714614)




