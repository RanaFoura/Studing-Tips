# Soft Delete
## Migrations
### $table->softDeletes();
## Model
### use Illuminate\Database\Eloquent\SoftDeletes;
### use SoftDeletes;
### protected $dates = ['deleted_at'];
## Controller
### banned (list of banned users)
> <pre>  public function banned()
> {
> $users = User::onlyTrashed()->get();
> return view('users.banned')->with('users',$users); 
> } </pre>
destroy (softdelete)
 public function destroy($id)
 {
 $user = User::find($id);
 $user->delete($id);
 return view('users.index')->with('users',User::all());
 }
hdelete (hard delete , permanently delete)
 public function hdelete($id)
 {
 $user = User::withTrashed()->where('id',$id)->first();
 $user->forceDelete();
 return redirect()->route('banned');
 }
restore (un ban)
 public function restore($id)
 {
 $user = User::withTrashed()->where('id',$id)->first();
 $user->restore();
 return redirect()->route('banned');
 }
route
banned
Route::get('/banned', [App\Http\Controllers\UserController::class, 'banned'])-
>name('banned');
/user/restore/{id}
Route::get('/user/restore/{id}', [App\Http\Controllers\UserController::class,
'restore'])->name('user.restore');
destroy
Route::delete('/user/destroy/{id}', [App\Http\Controllers\UserController::class,
'destroy'])->name('user.destroy');
hdelete
Route::get('/user/hdelete/{id}', [App\Http\Controllers\UserController::class,
'hdelete'])->name('user.hdelete');
