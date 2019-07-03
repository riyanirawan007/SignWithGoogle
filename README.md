# Sign With Google
Java utility for simplify Google Sign In on Android project, only one file .java you can start you google sign in with simpliest way.

## Requirements
- Activate your OAuth2.0 in [Google Developer Console](https://console.developers.google.com/)

## Gradle Dependencies
```gradle
    def gmsVersion="11.6.0"
    dependencies{
      implementation "com.google.android.gms:play-services-auth:${gmsVersion}"
    }
```    
## Example Usage
Make sure your package location has been added on top line of SignWithGoogle.java
```java
  private SignWithGoogle signWithGoogle;
  @Override
    public void onStart() {
         signWithGoogle=new SignWithGoogle(getContext()).build();
         super.onStart();
    }
  
  @Override
    public void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        signWithGoogle.onActivityResult(requestCode,resultCode,data);
    }
    
   public onClick(View v){
   
    if(v==btnSignInGoogle){
    //Do Sign In
    signWithGoogle.signIn(new SignWithGoogle.SignIn() {
                            @Override
                            public void doSignIn(@NonNull Intent SignInIntent, int REQUEST_CODE) {
                                startActivityForResult(SignInIntent,REQUEST_CODE);
                            }

                            @Override
                            public void after(@NonNull Status status, @Nullable GoogleSignInAccount account) {
                                Toast.makeText(getContext(), "Hello "+account.getDisplayName(), Toast.LENGTH_SHORT).show();
                            }
                        });
    }
    else if (v==btnSignOutGoogle){
    //Do Sign Out
    signWithGoogle.signOut(new SignWithGoogle.SignOut() {
                            @Override
                            public void before(@Nullable GoogleSignInAccount account) {

                            }

                            @Override
                            public void after(@NonNull Status status, @NonNull Task<Void> task) {
                                Toast.makeText(getContext(), "Logged out! status: "+status, Toast.LENGTH_SHORT).show();
                            }
                        });
    }
   
   }
  
```

# Disclaimer
- Modification <br> The whole code is designed by [@riyanirawan007](https://github.com@riyanirawan007), feel free to share and modify the code.
- Data Retrieved <br> You can get data from signed google acccount as explained on [GoogleSignInAccount API](https://developers.google.com/android/reference/com/google/android/gms/auth/api/signin/GoogleSignInAccount)
