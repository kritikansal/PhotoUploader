# -*- coding: utf-8 -*-
### required - do no delete
def user(): return dict(form=auth())
def download(): return response.download(request,db)
def call(): return service()

@auth.requires_membership('Admin')
def manage():
    db.image.User.default = auth.user.username
    db.like.User.default = auth.user.username
    db.comment.User.default=auth.user.username
    grid = SQLFORM.smartgrid(db.image)
    return dict(grid=grid)
    
@auth.requires_login()
def index():
    images = db().select(db.image.ALL, orderby=db.image.title)
    return dict(images=images)
@auth.requires_login()
def view():
    images = db().select(db.image.ALL, orderby=db.image.title)
    return dict(images=images)
@auth.requires_login()
def show():
    image = db.image(request.args(0)) or redirect(URL('index'))
    db.comment.image_id.default = image.id
    db.like.image_id.default = image.id
    db.like.User.default = auth.user.username
    db.comment.User.default=auth.user.username
    form = crud.create(db.comment,
                       message='your comment is posted',
            next=URL(args=image.id))
    form2 = crud.create(db.like,
                     message='you liked this photo',
            next=URL(args=image.id))
    comments = db(db.comment.image_id==image.id).select()
    likes = db(db.like.image_id==image.id).select()
    return dict(image=image, comments=comments, form=form ,likes=likes ,form2=form2)

@auth.requires_login()
def showdel():
   image = db.image(request.args(0)) or redirect(URL('index'))
   return dict(images=image)

@auth.requires_login()
def delete():
    #query = db(db.image.id==request.args(0)).select().first() ## grabbing comment to be deleted from comment_results
    remove = db(db.image.id==request.args(0)).delete()
    if remove:
         #response.flash = 'Image Deleted'
         redirect(URL('deleted'))
    else:
         #response.flash = 'Image Uploaded'
         redirect(URL('notdeleted'))
    return dict(remove=remove)


@auth.requires_login()
def deleted():
    return dict()

@auth.requires_login()
def notdeleted():
    return dict()
    
@auth.requires_login()
def calldelete():
    query=db.image.User==auth.user.username
    set=db(query)
    images=set.select()
    return dict(images=images)


@auth.requires_login()
def error():
    return dict()

@auth.requires_login()
def Upload():
   db.image.User.default=auth.user.username
   form = SQLFORM(db.image)
   if form.process().accepted:
       response.flash = 'Image Uploaded'
   elif form.errors:
       response.flash = 'form has errors'
   else:
       response.flash = 'Please Fill Details Correctly'
   return dict(form=form)
