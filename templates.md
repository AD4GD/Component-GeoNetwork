# Managing templates
This document talks about managing templates in GeoNetwork, as official instructions are really limited (https://docs.geonetwork-opensource.org/4.2/user-guide/describing-information/managing-templates/)

## Where we can access templates
The first thing you have to understand is that a template is a normal metadata record if an special tag. There are TWO "search" interfaces that look extremely similar but provide different functionalities:

The search interface:
![image](https://github.com/user-attachments/assets/81d79bd6-2135-474f-bc7e-df6a20bbb94e)

The editor borad interface
![image](https://github.com/user-attachments/assets/d398c8aa-7342-42e9-835a-cd2f58f86c3e)

Both have a left hand side list of filters but ONLY the second one provides a filter for templates
![image](https://github.com/user-attachments/assets/3157bcf6-1de9-48c5-babb-de0cdb0949cb)

This are the list of templates in AD4GD now
![image](https://github.com/user-attachments/assets/7ca40f81-b6cf-47f4-ac43-c2c960af05c2)

Note that this interface allos for removing unwanted templates in two ways.
* One by one 
![image](https://github.com/user-attachments/assets/8e421bed-c7e9-4e1e-8a09-ef380827144b)
* Some selected ones
![image](https://github.com/user-attachments/assets/b7fc827f-50cc-45fe-a03e-2fc452eb7111)

## Creation of a record based on a template
You can create a new metadata record from the [add record]. The same list of templates are available here.
![image](https://github.com/user-attachments/assets/3a7cbe04-a166-4b75-b755-94300605c993)

## Create a template based on a record
You can create a new template from a record in two steps.

First you need to duplicate the record by visiting it (no need to enter into edit mode):
![image](https://github.com/user-attachments/assets/0badb999-ae82-4e60-bc41-fe9a2d498b4e)

Then you can enter in edit mode and save the record as a template.
![image](https://github.com/user-attachments/assets/f7e91771-9868-4a9c-9405-65b1b039b805)

## Next steps: Minitemplates
I believe I have found the way to create fragments of metadata that can be used when editing metadata.

This interfaces in the edit mode:
![image](https://github.com/user-attachments/assets/18e2d6c7-7ee7-4aea-940d-c64f6d4931ec)

Seems to suggest that there is a way to create "minitemplates" of "contacts". I beleive this is managed here:
![image](https://github.com/user-attachments/assets/c565e490-02ef-4e31-8344-85ab60f0efbe)
But I need to explore this further.
