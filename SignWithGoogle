// package [your own package];

import android.content.Context;
import android.content.Intent;
import android.support.annotation.NonNull;
import android.support.annotation.Nullable;
import android.util.Log;

import com.google.android.gms.auth.api.signin.GoogleSignIn;
import com.google.android.gms.auth.api.signin.GoogleSignInAccount;
import com.google.android.gms.auth.api.signin.GoogleSignInClient;
import com.google.android.gms.auth.api.signin.GoogleSignInOptions;
import com.google.android.gms.common.api.ApiException;
import com.google.android.gms.common.api.CommonStatusCodes;
import com.google.android.gms.common.api.Status;
import com.google.android.gms.tasks.Task;

import static android.app.Activity.RESULT_OK;

public class SignWithGoogle {

    public final String ERROR_TAG="GOOGLE_SIGN_IN_DEBUG";
    private int REQUEST_CODE;
    public final int DEFAULT_REQUEST_CODE=77;
    private GoogleSignInClient client;
    private SignIn signInCallback;
    private Context context;


    public interface SignOut{
        void before(@Nullable GoogleSignInAccount account);
        void after(@NonNull Status status, @NonNull Task<Void> task);
    }

    public interface SignIn{
        void doSignIn(@NonNull Intent SignInIntent, final int REQUEST_CODE);
        void after(@NonNull Status status,@Nullable GoogleSignInAccount account);
    }


    public SignWithGoogle(@NonNull Context context)
    {
        super();
        this.context=context;
        this.REQUEST_CODE=DEFAULT_REQUEST_CODE;
    }

    public SignWithGoogle(@NonNull Context context, int REQUEST_CODE)
    {
        super();
        this.context=context;
        this.REQUEST_CODE=REQUEST_CODE;
    }

    public SignWithGoogle build(){
        GoogleSignInOptions options=new GoogleSignInOptions.Builder(GoogleSignInOptions.DEFAULT_SIGN_IN)
                .requestEmail()
                .requestProfile()
                .build();
        buildWithOptions(options);
        return this;
    }

    public void buildWithOptions(@NonNull GoogleSignInOptions options)
    {
        client= GoogleSignIn.getClient(context
                ,options);
    }

    public void signIn(@NonNull SignIn callback){
        setSignInCallback(callback);
        if(getSignedAccount()!=null) {
            signInCallback.after(new Status(CommonStatusCodes.SUCCESS),getSignedAccount());
        }
        else {
            Intent signInIntent=client.getSignInIntent();
            signInCallback.doSignIn(signInIntent,REQUEST_CODE);
        }
    }
    private void setSignInCallback(@NonNull SignIn callback)
    {
        signInCallback=callback;
    }

    public GoogleSignInAccount getSignedAccount() {
        return GoogleSignIn.getLastSignedInAccount(context);
    }

    public void signOut(@NonNull final SignOut callback) {
        GoogleSignInAccount account=GoogleSignIn.getLastSignedInAccount(context);
        callback.before(account);
        if(client!=null){
            Task<Void> task=client.signOut();
            callback.after(new Status(CommonStatusCodes.SUCCESS),task);
        }
    }


    public void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        if(resultCode==RESULT_OK)
        {
            if(requestCode==REQUEST_CODE)
            {
                Task<GoogleSignInAccount> task = GoogleSignIn.getSignedInAccountFromIntent(data);
                handleSignInResult(task);
            }
        }
    }

    private void handleSignInResult(Task<GoogleSignInAccount> task) {
        try {
            GoogleSignInAccount account = task.getResult(ApiException.class);
            if(signInCallback!=null)signInCallback.after(new Status(CommonStatusCodes.SUCCESS),account);
        } catch (ApiException e) {
            Log.e(ERROR_TAG, "signInResult:failed code=" + e.getStatusCode());
        }
    }


}
