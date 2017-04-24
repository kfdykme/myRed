package com.kfdykme.mered.myred;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import java.util.*;
import com.kfdykme.mered.myred.sugar.*;
import android.view.*;
import com.kfdykme.mered.myred.MyRedContract.Presenter;
import android.widget.*;
import com.kfdykme.mered.myred.view.*;
import android.widget.AdapterView.*;
import com.orm.*;

public class MyRedMain extends AppCompatActivity  implements MyRedContract.View
{

	@Override
	public void openEditItemDialog(android.view.View view)
	{

		LinearLayout l = (LinearLayout)view;
		TextView desText = (TextView)l.findViewById(R.id.DescText);
		String desc = desText.getText().toString();
		MyRed m =  mMyRedPresenter.findMyRedByDesc(desc);
		new EditDialog(MyRedMain.this,mMyRedPresenter).showEditDialog(m);
		
		// TODO: Implement this method
	}
	

	

	@Override
	public void openAddItemDialog()
	{
		new EditDialog(MyRedMain.this,mMyRedPresenter).showAddDialog();
		
		// TODO: Implement this method
	}

	
	private MyRedPresenter mMyRedPresenter;

	
	private ListView mListView;
	
	private Button mButton;
	
	@Override
	public void addItemList(List<MyRed> myReds)
	{
		if(myReds.size() != 0){

			ArrayList<HashMap<String,Object>> listItem = new ArrayList<HashMap<String,Object>>();

			for(int i = 0; i < myReds.size(); i++){
				HashMap<String,Object> map = new  HashMap<String,Object>();
				map.put("DescText",myReds.get(i).getDesc());
				map.put("WhoText",myReds.get(i).getWho());
				listItem.add(map);
			}

			SimpleAdapter mSimpleAdapter = new SimpleAdapter(
				this,
				listItem,
				R.layout.listview_item,
				new String[] {"DescText", "WhoText"},
				new int[] {R.id.DescText, R.id.WhoText});


			mListView.setAdapter(mSimpleAdapter);
			
	
			
			
			mListView.setOnItemClickListener(new OnItemClickListener(){
				
					@Override
					public void onItemClick(AdapterView<?> p1, android.view.View p2, int p3, long p4)
					{
						LinearLayout l = (LinearLayout)p2;
						TextView desText = (TextView)l.findViewById(R.id.DescText);
						String s = desText.getText().toString();
						mMyRedPresenter.visitUrl(s,MyRedMain.this);
						Toast.makeText(MyRedMain.this,s,Toast.LENGTH_SHORT).show();
						// TODO: Implement this method

					}
				});
				
			mListView.setOnItemLongClickListener(new OnItemLongClickListener(){

					@Override
					public boolean onItemLongClick(AdapterView<?> p1, android.view.View p2, int p3, long p4)
					{
						openEditItemDialog(p2);
						// TODO: Implement this method
						return false;
					}
				});
		}
		// TODO: Implement this method
	}
	

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
		//requestWindowFeature(Window.FEATURE_NO_TITLE);
        setContentView(R.layout.main);
		
		SugarContext.init(this);
			
		initView();
		
		upData();
		
    }

	public void upData()
	{
		if(mMyRedPresenter.loadMyRed() != null)
			addItemList(mMyRedPresenter.loadMyRed());
	}

	private void initView()
	{

		mMyRedPresenter = new MyRedPresenter(this);
		
		mListView = (ListView)findViewById(R.id.mainListView);
		
		mButton = (Button)findViewById(R.id.mainButton);
		
		mButton.setOnClickListener(new OnClickListener(){

				@Override
				public void onClick(android.view.View view)
				{
					openAddItemDialog();
						// TODO: Implement this method
				}
			});
		// TODO: Implement this method
	}

	@Override
	protected void onDestroy()
	{
		// TODO: Implement this method
		super.onDestroy();

		SugarContext.terminate();
		
	}
	
	

	public void setMyRedPresenter(MyRedPresenter mMyRedPresenter)
	{
		this.mMyRedPresenter = mMyRedPresenter;
	}

	public MyRedPresenter getMyRedPresenter()
	{
		return mMyRedPresenter;
	}
	
	
}
