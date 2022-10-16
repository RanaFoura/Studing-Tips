# Soft Delete
## Migrations
> ### <pre> $table->softDeletes(); </pre>


## Model
> ### <pre> use Illuminate\Database\Eloquent\SoftDeletes;</pre>
> ### <pre> use SoftDeletes; </pre>
> ### <pre> protected $dates = ['deleted_at']; </pre>


## Controller
> ### banned (list of banned users)
> <pre>  public function banned()
> {
> $users = User::onlyTrashed()->get();
> return view('users.banned')->with('users',$users); 
> } </pre>
> ### destroy (softdelete)
> <pre> public function destroy($id)
> {
> $user = User::find($id);
> $user->delete($id);
> return view('users.index')->with('users',User::all());
> } </pre>
> ### hdelete (hard delete , permanently delete)
> <pre>  public function hdelete($id)
> {
> $user = User::withTrashed()->where('id',$id)->first();
> $user->forceDelete();
> return redirect()->route('banned');
> } </pre>
> ### restore (un ban)
> <pre>  public function restore($id)
> {
> $user = User::withTrashed()->where('id',$id)->first();
> $user->restore();
> return redirect()->route('banned');
> } </pre>


## route
> ### banned
> <pre> Route::get('/banned', [App\Http\Controllers\UserController::class, 'banned'])->name('banned');</pre>
> ### restore
> <pre> Route::get('/user/restore/{id}', [App\Http\Controllers\UserController::class,'restore'])->name('user.restore'); </pre>
> ### destroy
> <pre> Route::delete('/user/destroy/{id}', [App\Http\Controllers\UserController::class,'destroy'])->name('user.destroy');</pre>
> ### hdelete
> <pre> Route::get('/user/hdelete/{id}', [App\Http\Controllers\UserController::class,'hdelete'])->name('user.hdelete'); </pre

# Mindmap for Soft Delete
> ![Soft Delete](https://user-images.githubusercontent.com/115618347/196014027-835d3163-6f20-420c-8b25-7740aed7c5e7.png)
> [XMIND URL](https://xmind.works/share/Vl3V50TE)
