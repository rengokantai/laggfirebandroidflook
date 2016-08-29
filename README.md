#### laggfirebandroidflook
######Overview of Firebase Remote Config
######Understand parameters and conditions
######Implement Firebase Remote Config
create xml file under res/xml called ke.xml
```
<defaultsMap>
<enrty>
<key>
</key>
<value>
</value>
</entry>
</defaultsMap>
```
in MainActivity.xml
```
private FirebaseRemoteConfig mFBConfig;

protected void onCreate(Bundle ..){
	mFBConfig =FirebaseRemoteConfig.getInstance();
}


//Add remote config settings
FirebaseRemoteConfigSettings cs = new FirebaseRemoteConfigSettings.Builder()
.setDeveloperMpdeEnabled(BuildConfig.DEBUG).build();
mFBConfig.setConfigSettings(cs);
mFBConifg.setDefaults(R.xml.ke);


```





######Implement email and password authentication
```
FirebaseAuth.getInstance()
FirebaseAuth.AuthStateListener()
createUserWithEmailAndPassword(username,password)
signInWithEmailAndPassword(username,password)
signOut()
```
Add in build.gradle
```
compile 'com.google.firebase:firebase-auth:9.0.2'
```

SignActivity.java
```
private FirabaseAuth mAuth;
private FirebaseAuth.AuthStateListener mAuthListener;

mAuth = FirebaseAuth.getInstance();
mAuthListener = new FirebaseAuth.AuthStateListener(){
	@Override
	public void onAuthStateChange(@NonNull FirebaseAuth firebaseauth){

	FirebaseUser user = firebaseAuth.getCurrentUser();
if(user!=null){
	Log.d(TAG, "Signed in: "+ user.getId());
}else{
	Log.d(TAG,"Signed out");
}

}
}

@Override
public void onStart(){
	super.onStart();
	mAuth.addAuthStateListener(mAuthListener);
}


@Override
public void onStop(){
	super.onStop();
if(mAuthListener!=null){
	mAuth.removeAuthStateListener(mAuthListener);
}
}


@Override
public void onClick(View v){
	switch(v.getId()){
	case R.id.betSignIn;
	signUserIn();
	break;
}
}



private void createUserAccount(){
	mAuth.createUserWithEmailAndPassword(email,password)
.addOnCompleteListener(this,new OnCompleteListener<AuthResult>(){
	@Override
	public void onComplete(@NonNull Task<AuthResult> task){

if(task.isSuccessful()){
Toast.makeText(SignInActivity.this,"created",Toast.LENGTH_SHORT).show();
}else{

Toast.makeText(SignInActivity.this,"failed",Toast.LENGTH_SHORT).show();
}
}
}
});
}
}
```

######Add Firebase Auth error handling
```
addOnFailureListener();
FirebaseAuthInvalidCreadentialsException
FirebaseAuthInvalidUserException
FirebaseAuthUserCollisionException
```
last code, in loginAccoubt()
```
.addOnFailureListener(new OnFailureListener(){
	@Override
	public void onFailure(@NonNull Exception e){
	if (e instanceof FirebaseAuthInvalidCredentialsException){
	update("wrong pass")
}else if (e instanceof FirebaseAuthInvalidUserException){
update("no account with this email")
}else{
update(e.getLocalizedMessage());
}
});
}

```
in createUserAccount
```
.addOnFailureListener(new OnFailureListener(){
	@Override
	public void onFailure(@NonNull Exception e){
if (e instanceof FirebaseAuthUserCollisionException)
```
