---
layout: "post"
title: "TestHashMap"
date: "2018-08-20 01:26"
---

```
package jp.mori;

import java.util.*;
import java.util.Map.Entry;

public class UtilTest {

	Scanner scan = new Scanner(System.in);
	HashMap<String, cdDTO> hmap;
	static int rembang;
	cdDTO deleted;
	cdDTO result;

	public UtilTest() {
		hmap = new HashMap<String, cdDTO>();
		rembang = 1;
		deleted = null;
		result = null;
	}

	public HashMap<String, cdDTO> putHashMap( HashMap<String, cdDTO> hmap){

		return hmap;
	}

	boolean exitCode = true;
	public void operation(int a) {

	while(exitCode) {

		System.out.print("input:1
    , delete:2
    , search:3
    , selectAll:4
    , EXIT:5
    , Recover:9 >>");

		int pattern = scan.nextInt();
				switch(pattern) {
				case 0:
					scan.reset();
					operation(pattern);
				case 1:
					cdDTO dto = new cdDTO();

					putHashMap(hmap).put(String.valueOf(rembang), dto);
					rembang ++;
					dto.tableMap.putAll(hmap);
					scan.reset();
					break;
				case 2:
					System.out.println("Tablemap  = " + dto.getTableMap() + "\n");
					System.out.print("INPUT CODE(rembang) >>");
					String del_code = scan.next();
					System.out.print("Really delete? (Y/N) >>" );
					Scanner scan3 = new Scanner(System.in);
					String reDel = scan3.nextLine();
					if(reDel.equals("Y")||reDel.equals("y")) {
						try {
							deleted = putHashMap(hmap).remove(del_code);
						}catch(Exception e) {
							System.out.println(del_code + " is not exist");
						}
					}else if(reDel.equals("N")||reDel.equals("n")) {
						break;
					}
					System.out.println( deleted + " is deleted");
					scan.reset();
					break;

				case 3:
					System.out.println("Tablemap  = " + dto.getTableMap() + "\n");
					//System.out.print("SEARCH CODE(rembang) >>");
					//String get_rembang = scan.next();
					//result = putHashMap(hmap).get(get_rembang);
					//System.out.println("==> "+putHashMap(hmap).get(get_rembang));
					//int get_kubun = 1;

					HashMap<String, cdDTO> reMap = new HashMap<String, cdDTO>();
					Collection<cdDTO> result2 = putHashMap(hmap).values();
					Iterator<cdDTO> itr2 = result2.iterator();
					while(itr2.hasNext()) {
						cdDTO dto2 = itr2.next();
						System.out.println(dto2);
						reMap.put(dto2.ZAIRYU_SHIKAKU_CD, dto2);
					}
					System.out.println(reMap);
					System.out.print("SEARCH ZAIRYU_CODE >>");
					String get_kubun = scan.next();
					System.out.println(">>" + reMap.get(get_kubun));

					ArrayList<cdDTO> list = new ArrayList<>();
					for(Entry<String, cdDTO> resultSet : putHashMap(hmap).entrySet()) {
						list.add(resultSet.getValue());
					}
					//System.out.println(list.get(0));


					for(Entry<String, cdDTO> entrySet : putHashMap(hmap).entrySet()) {
						System.out.println("Key = " + entrySet.getKey()
                            +" Value = "+ entrySet.getValue());
					}
					scan.reset();
					break;
				case 4:
					System.out.println("All Count = " + putHashMap(hmap).size());
					System.out.println("Tablemap  = " + dto.getTableMap());
					Set<String> entrySet = putHashMap(hmap).keySet();
					Iterator<String> itr = entrySet.iterator();
					while(itr.hasNext()) {
						String str = itr.next();
						System.out.println(putHashMap(hmap).get(str));
					}
					scan.reset();
					break;
				case 5:
					exitMethod(a);
					scan.reset();
					exitCode = false;

				case 9:
					System.out.println(" >> Deleted = "+ deleted);
					System.out.print("Really Recover? (Y/N) >>" );
					Scanner scan4 = new Scanner(System.in);
					String reInput = scan4.nextLine();

					if(reInput.equals("Y")||reInput.equals("y")) {
						HashMap<String, cdDTO> newMap = new HashMap<String, cdDTO>();
						newMap.put(String.valueOf(rembang+1), deleted);
						putHashMap(hmap).putAll(newMap);
						entrySet = putHashMap(hmap).keySet();
						itr = entrySet.iterator();
						while(itr.hasNext()) {
							String str = itr.next();
							System.out.println(putHashMap(hmap).get(str));
						}
					}else if(reInput.equals("N")||reInput.equals("n")) {
						break;
					}

					rembang ++;
					scan4.reset();
					scan.reset();
					break;
				}

				pattern = a;
				scan.reset();
			}
	}

	public void exitMethod(int a) {
		System.out.print("Exit the program? (Y/N) >>" );
		Scanner scan2 = new Scanner(System.in);
		String exit = scan2.nextLine();
		if(exit.equals("Y")||exit.equals("y")) {
			//System.exit(0);
			scan2.reset();
			exitCode = false;
			//operation(6);
		}else if(exit.equals("N")||exit.equals("n")) {
			operation(0);
		}
	}

}
```
