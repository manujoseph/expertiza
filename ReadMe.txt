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

2. The self.export method contains a lot of statements that have been repeated.
Refactor this method to avoid duplication.

The method exports the scores of teams and their corresponding participants into a CSV.


Refactoring done
________________

In this function, there was repetition of code to check if an option is selected.
If the option was selected and a corresponding score existed then it was added to
the csv to be exported.

We moved the repeated code to a function and then reused the function. Also, a few
variables were renamed for better code clarity and readability.


Verification
____________

The diff can be viewed in the link below :-

https://github.com/manujoseph/expertiza/commit/df3bdee3943af294a1d739bfb156a89b9257e17b

The export assignment scores functionality of expertiza is not broken.

Note: The logic that existed has a bug. It does not export the score if the assignment had only a single team.
Since this is a refactoring project we did not fix bug or change the existing functionality.

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

Readme file and/or other documentation. Say what project you are doing, and preferably, give a link to the project description, so your reviewers will know how to evaluate you.  If you are doing a testing-only project, give instructions on how to run the tests.

4. The 'get_scores' method in the 'assignment', 'assignment_participant' and 'assignment_team'
files contain lines of code that appear to be repeated. Can this be refactored so that
only one method implements the common functionality?

The get_scores method returns the score of a paritcipant for the given questions.

Refactoring done
________________
The get_scores method returns the score of a particular paritcipant for the given questions.
So it is more appropriate to have the function implemented as an instance method of the
Participant model.

Removed the function from required in AssignmentParticipant, since it inherits Participant,
and Participant already implements the get_scores method.

Removed the repeated code which gets the score for all participants in the assignment.
The repeated code was removed and replaced my a call to the get_scores instance method
of participant.


Verification
____________

The diff can be viewed in the link below :-

https://github.com/manujoseph/expertiza/commit/b0c43755e8914059461a2b8e220db61a63dd6392

The scores of the participants in an assignment continues to be displayed in expertiza.

5. Refactor 'get_hyperlinks' to replace the conditional statement with polymorphism. (see get_hyperlinks method in assignment_team).
- The get_hyperlinks method is used to get all the submission links associated with the particular assignment by a particular user/team. Hence, in the earlier method, get_hyperlinks would be called by either a team or an individual participant object based upon the nature of the assignment.

Refactoring done
________________
- We add a simple logic check called get_members to find the type of the member for the particular assignment type. Just like the get_hyperlinks method in assignment_team, we refactor it to salvage polymorphism and have a simple for loop get all the related links according to the object type returned by get_members.


Verification
____________

Code can be verified in the github commit:
https://github.com/manujoseph/expertiza/commit/0f1761e226b354467f2f981b88a7aff4952f218b

For testing purposes, any assignment submission page would reflect the changes and function normally/as before.


6. Refactor the 'get_reviews' method by replacing the conditional statement with polymorphism.
- The get_reviews method is used to get all the reviews for a particular assignment (team or individual) for the instructor/admin to view. This function is alternatively also used at many other places to aggregate and display the reviews for the particular assignment. The function uses a conditional block to test if the assignment is group or invididual and accordingly calls the appropriate ResponseMap object, which eitherways calls get_assessment_for(). Also again, the parameter passed to the get_assessment_for() function is decided based on the type of the assignment which is either group or individual.

Refactoring done
________________
- We again impart polymorphism by using a generic ResponseMap object to call the get_assessment_for() method. Again, we try to have a simple logic block outisde which determines the participant type and calls the get_reviews methods.

Verification
____________

Code can be verified in the github commit:
https://github.com/manujoseph/expertiza/commit/0f1761e226b354467f2f981b88a7aff4952f218b

For testing purposes, with the admin login, one can check for grades and reviews on any given assignment to gauge the impact.

8. Also apply other refactorings such as Rename variable, Rename method to give the variables and methods more meaningful names

Typo in submit_hyperlinks method in assignment_participant.rb corrected (commit: https://github.com/manujoseph/expertiza/commit/374d442b3a4867531877e9755c2ab14c30714614)

renamed 'pscore' to 'participant_score' and 'tcsv' to 'participant_record' in the self.export of assignment.rb



